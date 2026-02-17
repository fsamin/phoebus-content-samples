---
title: "The Control Plane"
type: lesson
estimated_duration: "20m"
---

# The Kubernetes Control Plane

## Cluster Architecture

A Kubernetes cluster has two types of nodes:

```
┌─── Control Plane ────────────────┐   ┌─── Worker Nodes ─────────────┐
│                                  │   │                               │
│  API Server ◄──► etcd            │   │  kubelet                      │
│      │                           │   │    │                          │
│  Scheduler    Controller Manager │   │  Container Runtime (containerd)│
│                                  │   │    │                          │
│  Cloud Controller (optional)     │   │  kube-proxy                   │
│                                  │   │    │                          │
└──────────────────────────────────┘   │  Pods [container] [container] │
                                       └───────────────────────────────┘
```

## Control Plane Components

### API Server (`kube-apiserver`)

The front door to Kubernetes. Every `kubectl` command, every Helm release, every CI/CD deployment talks to the API server.

- RESTful API over HTTPS
- Authentication, authorization, admission control
- The only component that talks to etcd

### etcd

A distributed key-value store that holds the **entire cluster state**:
- Pod specifications
- Service endpoints
- ConfigMaps and Secrets
- RBAC policies

> If etcd is lost and there's no backup, you lose the cluster.

### Scheduler (`kube-scheduler`)

Decides **which node** a new pod runs on based on:
- Resource requests (CPU, memory)
- Node affinity/anti-affinity rules
- Taints and tolerations
- Data locality

### Controller Manager (`kube-controller-manager`)

Runs control loops that watch the cluster state and make changes:
- **ReplicaSet controller**: ensures the right number of pod replicas
- **Deployment controller**: manages rolling updates
- **Node controller**: monitors node health
- **Job controller**: manages batch jobs

## Worker Node Components

### kubelet

The agent on each node that:
- Receives pod specifications from the API server
- Manages container lifecycle via the container runtime
- Reports node and pod status back

### Container Runtime

The software that actually runs containers:
- **containerd** (default, recommended)
- **CRI-O** (alternative)
- Docker was removed as a runtime in Kubernetes 1.24

### kube-proxy

Maintains network rules on nodes:
- Routes traffic to the correct pods
- Implements Kubernetes Services (ClusterIP, NodePort, LoadBalancer)

## kubectl — Your CLI Tool

```bash
# Cluster info
kubectl cluster-info
kubectl get nodes

# Namespace management
kubectl get namespaces
kubectl create namespace staging

# Context switching
kubectl config get-contexts
kubectl config use-context production

# Quick debug
kubectl get events --sort-by='.lastTimestamp'
kubectl top nodes
kubectl top pods
```

## Summary

The control plane is the brain of Kubernetes: API Server receives requests, Scheduler places pods, Controllers ensure desired state, and etcd stores everything. Worker nodes run your workloads through kubelet and the container runtime.
