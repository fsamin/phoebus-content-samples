---
title: "Health Checks & Monitoring"
type: lesson
estimated_duration: "20m"
---

# Health Checks & Monitoring

## Why Health Checks?

Without health checks, Kubernetes doesn't know if your application is actually working. It only knows if the process is running. A process can be running but:
- Stuck in a deadlock
- Unable to connect to the database
- Serving 500 errors

## Probe Types

### Liveness Probe

**"Is the process alive?"** If it fails, Kubernetes kills and restarts the container.

```yaml
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 15
  periodSeconds: 10
  failureThreshold: 3
```

**Use for:** Detecting deadlocks, infinite loops, corrupted state.

### Readiness Probe

**"Is the pod ready to receive traffic?"** If it fails, the pod is removed from the Service endpoints (no traffic routed to it).

```yaml
readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 5
  failureThreshold: 3
```

**Use for:** Warming up caches, waiting for DB connection, loading large datasets.

### Startup Probe

**"Has the app finished starting?"** Disables liveness/readiness probes until it succeeds.

```yaml
startupProbe:
  httpGet:
    path: /healthz
    port: 8080
  failureThreshold: 30
  periodSeconds: 10
```

**Use for:** Slow-starting applications (JVM, large datasets to load).

## Probe Methods

| Method | Example | Best For |
|--------|---------|----------|
| HTTP GET | `httpGet: path: /health` | Web applications |
| TCP Socket | `tcpSocket: port: 5432` | Databases, raw TCP services |
| Exec command | `exec: command: ["pg_isready"]` | Custom health logic |
| gRPC | `grpc: port: 50051` | gRPC services |

## Implementing Health Endpoints

### Go example

```go
// /healthz — liveness: is the process alive?
func handleHealthz(w http.ResponseWriter, r *http.Request) {
    w.WriteHeader(http.StatusOK)
    w.Write([]byte("ok"))
}

// /ready — readiness: can we serve traffic?
func handleReady(w http.ResponseWriter, r *http.Request) {
    if err := db.Ping(); err != nil {
        w.WriteHeader(http.StatusServiceUnavailable)
        w.Write([]byte("database unavailable"))
        return
    }
    w.WriteHeader(http.StatusOK)
    w.Write([]byte("ready"))
}
```

## Monitoring with Prometheus

### Application metrics

Expose metrics at `/metrics` in Prometheus format:

```
# HELP http_requests_total Total HTTP requests
# TYPE http_requests_total counter
http_requests_total{method="GET",path="/api/users",status="200"} 1234

# HELP http_request_duration_seconds HTTP request duration
# TYPE http_request_duration_seconds histogram
http_request_duration_seconds_bucket{le="0.01"} 980
http_request_duration_seconds_bucket{le="0.1"} 1200
http_request_duration_seconds_bucket{le="1.0"} 1234
```

### ServiceMonitor (for Prometheus Operator)

```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: api-monitor
spec:
  selector:
    matchLabels:
      app: api
  endpoints:
    - port: http
      path: /metrics
      interval: 15s
```

## Resource Management

Always set resource requests and limits:

```yaml
resources:
  requests:
    cpu: 100m        # Guaranteed minimum
    memory: 128Mi
  limits:
    cpu: 500m        # Hard ceiling
    memory: 256Mi
```

### What happens when limits are exceeded?

| Resource | Exceeds Limit | Result |
|----------|--------------|--------|
| CPU | Over limit | Throttled (slower, not killed) |
| Memory | Over limit | OOMKilled (container killed and restarted) |

## Useful Monitoring Commands

```bash
# Resource usage
kubectl top nodes
kubectl top pods

# Events (debugging)
kubectl get events --sort-by='.lastTimestamp'
kubectl get events --field-selector type=Warning

# Describe pod (shows events, probes, restarts)
kubectl describe pod api-5d94cf4b7c-k2j8m

# Pod logs
kubectl logs api-5d94cf4b7c-k2j8m
kubectl logs api-5d94cf4b7c-k2j8m --previous  # Logs from crashed container
kubectl logs -l app=api --all-containers        # All pods matching label
```

## Summary

Health checks tell Kubernetes when your app is alive, ready, and starting. Monitoring with Prometheus gives you visibility into performance. Resource limits prevent a single pod from consuming the entire node. Together, they form the foundation of production-grade operations.
