---
title: "Kubernetes Architecture Quiz"
type: quiz
estimated_duration: "10m"
---

# Kubernetes Architecture Quiz

## [multiple-choice] What is the smallest deployable unit in Kubernetes?

- [ ] Container
- [x] Pod
- [ ] Deployment
- [ ] Node

> **Explanation:** A Pod is the smallest unit. It contains one or more containers that share the same network namespace and storage. Containers within a pod share the same IP and can communicate via localhost.

## [multiple-choice] Which component stores the entire cluster state?

- [ ] API Server
- [ ] Scheduler
- [x] etcd
- [ ] kubelet

> **Explanation:** etcd is a distributed key-value store that holds all cluster state — pods, services, secrets, RBAC policies. The API Server is the only component that reads/writes to etcd directly.

## [short-answer] What command scales a deployment named "api" to 5 replicas?

    kubectl scale deployment api --replicas=5

> **Explanation:** `kubectl scale` adjusts the replica count of a deployment. The Deployment controller then creates or removes pods to match the desired count.

## [multiple-choice] What is the difference between ClusterIP and LoadBalancer service types?

- [x] ClusterIP is internal only; LoadBalancer provisions an external cloud load balancer
- [ ] ClusterIP is faster; LoadBalancer is more reliable
- [ ] ClusterIP uses TCP; LoadBalancer uses HTTP
- [ ] There is no difference in production

> **Explanation:** ClusterIP (default) creates an internal IP reachable only within the cluster. LoadBalancer tells the cloud provider to provision an external load balancer with a public IP that routes to your service.

## [multiple-choice] Why should you use Deployments instead of creating Pods directly?

- [ ] Pods can't have resource limits
- [ ] Deployments are faster
- [x] Deployments provide self-healing, rolling updates, and replica management
- [ ] Pods are deprecated

> **Explanation:** Deployments manage pod lifecycle: they restart crashed pods, support rolling updates for zero-downtime deployments, and maintain the desired replica count. Standalone pods have none of these capabilities.
