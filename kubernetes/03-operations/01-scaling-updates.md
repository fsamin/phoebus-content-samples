---
title: "Scaling & Rolling Updates"
type: lesson
estimated_duration: "20m"
---

# Scaling & Rolling Updates

## Horizontal Scaling

### Manual Scaling

```bash
# Scale up
kubectl scale deployment api --replicas=5

# Scale down
kubectl scale deployment api --replicas=2

# Check current state
kubectl get deployment api
```

### Horizontal Pod Autoscaler (HPA)

Automatically scale based on CPU/memory usage:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: api-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: api
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
    - type: Resource
      resource:
        name: memory
        target:
          type: Utilization
          averageUtilization: 80
```

```bash
# Create HPA from command line
kubectl autoscale deployment api --min=2 --max=10 --cpu-percent=70

# View HPA status
kubectl get hpa
```

> **Prerequisite:** metrics-server must be installed for HPA to work.

## Rolling Updates

Deployments update pods gradually with zero downtime.

### Update Strategy

```yaml
spec:
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1         # Max pods above desired count during update
      maxUnavailable: 0   # No downtime — always maintain desired count
```

### Performing an Update

```bash
# Update the image
kubectl set image deployment/api api=mycompany/api:1.3.0

# Watch the rollout
kubectl rollout status deployment/api

# View history
kubectl rollout history deployment/api

# Rollback to previous version
kubectl rollout undo deployment/api

# Rollback to specific revision
kubectl rollout undo deployment/api --to-revision=2

# Pause/Resume a rollout
kubectl rollout pause deployment/api
kubectl rollout resume deployment/api
```

### How It Works

With `maxSurge=1` and `maxUnavailable=0` (3 replicas):

```
Step 1: 3 old pods running
Step 2: 1 new pod created (4 total, 3 old + 1 new)
Step 3: New pod ready → 1 old pod terminated (3 total)
Step 4: 1 new pod created (4 total, 2 old + 2 new)
Step 5: New pod ready → 1 old pod terminated (3 total)
Step 6: 1 new pod created (4 total, 1 old + 3 new)
Step 7: New pod ready → last old pod terminated (3 new pods)
```

## Blue-Green and Canary

### Blue-Green

Deploy new version alongside old, switch traffic at once:

```bash
# Deploy "green" version
kubectl apply -f deployment-green.yaml

# Test green version
kubectl port-forward svc/api-green 8081:80

# Switch service to green
kubectl patch svc api -p '{"spec":{"selector":{"version":"green"}}}'

# Remove blue version
kubectl delete deployment api-blue
```

### Canary

Route a small percentage of traffic to the new version:

```yaml
# Main deployment (90% traffic)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api
spec:
  replicas: 9
  template:
    metadata:
      labels:
        app: api

# Canary deployment (10% traffic)
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-canary
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: api        # Same label → same service
```

Both deployments share the same Service. Traffic is distributed based on pod count.

## Summary

- **HPA** for automatic scaling based on metrics
- **Rolling updates** for zero-downtime deployments
- **Rollbacks** for instant recovery
- **Blue-green/canary** for controlled, safe releases
