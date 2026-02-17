---
title: "Persistent Volumes"
type: lesson
estimated_duration: "20m"
---

# Persistent Volumes

## The Problem

Containers are ephemeral. When a pod restarts, its filesystem resets. Databases, file uploads, and logs need **persistent storage** that survives pod restarts and rescheduling.

## Storage Architecture

```
PersistentVolume (PV)     ← Cluster admin creates (or dynamic provisioning)
       ↕
PersistentVolumeClaim (PVC) ← Developer requests
       ↕
Pod volume mount           ← App uses the storage
```

### PersistentVolume (PV)

A piece of storage in the cluster, provisioned by an admin or dynamically:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: postgres-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: ssd
  hostPath:
    path: /data/postgres   # For local development only
```

### PersistentVolumeClaim (PVC)

A request for storage by a developer:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: postgres-data
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
  storageClassName: ssd
```

### Using PVC in a Pod

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: postgres
spec:
  replicas: 1
  template:
    spec:
      containers:
        - name: postgres
          image: postgres:17-alpine
          volumeMounts:
            - name: data
              mountPath: /var/lib/postgresql/data
      volumes:
        - name: data
          persistentVolumeClaim:
            claimName: postgres-data
```

## Access Modes

| Mode | Short | Description |
|------|-------|-------------|
| ReadWriteOnce | RWO | Read-write by a single node |
| ReadOnlyMany | ROX | Read-only by multiple nodes |
| ReadWriteMany | RWX | Read-write by multiple nodes |

Most cloud block storage only supports RWO. For RWX, use NFS or distributed storage (Ceph, EFS).

## Reclaim Policies

| Policy | Behavior |
|--------|----------|
| `Retain` | PV is kept after PVC deletion (manual cleanup) |
| `Delete` | PV and underlying storage are deleted |
| `Recycle` | Deprecated — PV is wiped and reused |

## Dynamic Provisioning

With a `StorageClass`, PVs are created automatically when a PVC is created:

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: fast-ssd
provisioner: kubernetes.io/gce-pd
parameters:
  type: pd-ssd
reclaimPolicy: Delete
volumeBindingMode: WaitForFirstConsumer
```

```yaml
# Just create a PVC — the PV is auto-provisioned
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: app-data
spec:
  storageClassName: fast-ssd
  accessModes: [ReadWriteOnce]
  resources:
    requests:
      storage: 50Gi
```

## Volume Types

| Type | Use Case |
|------|----------|
| `emptyDir` | Temporary scratch space (deleted with pod) |
| `hostPath` | Development only (not portable) |
| `nfs` | Shared storage across pods |
| `awsElasticBlockStore` | AWS EBS volumes |
| `gcePersistentDisk` | GCP persistent disks |
| `azureDisk` | Azure managed disks |
| `csi` | Any storage via Container Storage Interface |

## Summary

PVs provide the storage, PVCs request it, and pods mount it. Use dynamic provisioning in production. Always set a reclaim policy, and prefer cloud-native CSI drivers.
