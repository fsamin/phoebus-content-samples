---
title: "ConfigMaps & Secrets"
type: lesson
estimated_duration: "20m"
---

# ConfigMaps & Secrets

## The Problem

Hardcoding configuration in container images breaks the **12-factor app** principle. You'd need to rebuild the image for every environment.

**Solution:** Externalize configuration with ConfigMaps (non-sensitive) and Secrets (sensitive).

## ConfigMaps

Store non-sensitive configuration: feature flags, URLs, application settings.

### Create from literal values

```bash
kubectl create configmap app-config \
    --from-literal=LOG_LEVEL=info \
    --from-literal=API_URL=https://api.internal
```

### Create from file

```bash
kubectl create configmap nginx-conf --from-file=nginx.conf
```

### ConfigMap manifest

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  LOG_LEVEL: "info"
  API_URL: "https://api.internal"
  MAX_CONNECTIONS: "100"
  config.yaml: |
    database:
      host: postgres
      port: 5432
    cache:
      host: redis
      port: 6379
```

### Using ConfigMaps in Pods

#### As environment variables

```yaml
spec:
  containers:
    - name: app
      envFrom:
        - configMapRef:
            name: app-config
      # Or individual keys:
      env:
        - name: LOG_LEVEL
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: LOG_LEVEL
```

#### As mounted files

```yaml
spec:
  containers:
    - name: app
      volumeMounts:
        - name: config-volume
          mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        name: app-config
```

## Secrets

Store sensitive data: passwords, API keys, TLS certificates.

> ⚠️ Kubernetes Secrets are **base64-encoded, not encrypted** by default. Enable encryption at rest in production.

### Create a secret

```bash
kubectl create secret generic db-credentials \
    --from-literal=username=admin \
    --from-literal=password='s3cur3P@ss!'
```

### Secret manifest

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-credentials
type: Opaque
data:
  username: YWRtaW4=           # base64 encoded "admin"
  password: czNjdXIzUEBzcyE=  # base64 encoded
```

Or use `stringData` to avoid manual base64:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-credentials
type: Opaque
stringData:
  username: admin
  password: "s3cur3P@ss!"
```

### Using Secrets in Pods

```yaml
spec:
  containers:
    - name: app
      env:
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db-credentials
              key: password
      volumeMounts:
        - name: tls-certs
          mountPath: /etc/tls
          readOnly: true
  volumes:
    - name: tls-certs
      secret:
        secretName: tls-secret
```

## Best Practices

1. **Never commit Secrets to Git** — use sealed-secrets, SOPS, or external secret managers
2. **Enable encryption at rest** for etcd
3. **Use RBAC** to restrict Secret access to specific namespaces/service accounts
4. **Rotate Secrets** regularly
5. **Use ConfigMaps for non-sensitive data** — don't put everything in Secrets

## Summary

ConfigMaps and Secrets separate configuration from code. ConfigMaps for non-sensitive data, Secrets for credentials and keys. Both can be consumed as environment variables or mounted files.
