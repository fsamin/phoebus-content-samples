---
title: "Pods, Services & Deployments"
type: lesson
estimated_duration: "25m"
---

# Pods, Services & Deployments

## Pods

A **Pod** is the smallest deployable unit in Kubernetes. It's a group of one or more containers that:
- Share the same network namespace (same IP, same ports)
- Share storage volumes
- Are always co-scheduled on the same node

### Pod manifest

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  containers:
    - name: nginx
      image: nginx:1.25-alpine
      ports:
        - containerPort: 80
      resources:
        requests:
          cpu: 100m
          memory: 128Mi
        limits:
          cpu: 200m
          memory: 256Mi
```

### Pod lifecycle

```
Pending → Running → Succeeded/Failed
              │
          ContainerCreating → Running → Terminated
```

> You rarely create Pods directly. Use Deployments instead.

## Deployments

A **Deployment** manages a set of identical pods (via a ReplicaSet). It handles:
- Desired replica count
- Rolling updates and rollbacks
- Self-healing (restarts crashed pods)

### Deployment manifest

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api
  labels:
    app: api
spec:
  replicas: 3
  selector:
    matchLabels:
      app: api
  template:
    metadata:
      labels:
        app: api
    spec:
      containers:
        - name: api
          image: mycompany/api:1.2.0
          ports:
            - containerPort: 8080
          env:
            - name: DATABASE_URL
              valueFrom:
                secretKeyRef:
                  name: db-credentials
                  key: url
          resources:
            requests:
              cpu: 100m
              memory: 128Mi
            limits:
              cpu: 500m
              memory: 256Mi
          livenessProbe:
            httpGet:
              path: /health
              port: 8080
            initialDelaySeconds: 10
            periodSeconds: 10
          readinessProbe:
            httpGet:
              path: /ready
              port: 8080
            initialDelaySeconds: 5
            periodSeconds: 5
```

### Key commands

```bash
# Create
kubectl apply -f deployment.yaml

# View
kubectl get deployments
kubectl describe deployment api

# Scale
kubectl scale deployment api --replicas=5

# Update image
kubectl set image deployment/api api=mycompany/api:1.3.0

# Rollback
kubectl rollout undo deployment/api

# View rollout history
kubectl rollout history deployment/api

# Watch rollout
kubectl rollout status deployment/api
```

## Services

A **Service** provides a stable network endpoint for a set of pods. Pods are ephemeral — their IPs change. Services give them a permanent address.

### Service types

| Type | Description | Access |
|------|-------------|--------|
| `ClusterIP` | Internal cluster IP (default) | Within cluster only |
| `NodePort` | Port on every node | External via `<NodeIP>:<NodePort>` |
| `LoadBalancer` | Cloud load balancer | External via cloud LB IP |
| `ExternalName` | DNS alias | Redirects to external service |

### ClusterIP Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: api-service
spec:
  type: ClusterIP
  selector:
    app: api
  ports:
    - port: 80            # Service port
      targetPort: 8080    # Container port
      protocol: TCP
```

Other pods can reach this service at `api-service:80` or `api-service.default.svc.cluster.local:80`.

### LoadBalancer Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: api-public
spec:
  type: LoadBalancer
  selector:
    app: api
  ports:
    - port: 443
      targetPort: 8080
```

### Service discovery

```bash
# From any pod in the cluster:
curl http://api-service                    # Same namespace
curl http://api-service.staging            # Different namespace
curl http://api-service.staging.svc.cluster.local  # Full DNS
```

## Namespaces

Namespaces provide logical isolation within a cluster:

```bash
kubectl create namespace production
kubectl create namespace staging

# Deploy to a specific namespace
kubectl apply -f deployment.yaml -n production

# Set default namespace
kubectl config set-context --current --namespace=production
```

## Summary

- **Pods** = smallest deployable unit, rarely created directly
- **Deployments** = manage pod replicas, rolling updates, self-healing
- **Services** = stable network endpoint for a set of pods
- **Namespaces** = logical isolation for multi-tenant or multi-environment clusters
