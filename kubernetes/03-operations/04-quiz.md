---
title: "Operations Quiz"
type: quiz
estimated_duration: "10m"
---

# Kubernetes Operations Quiz

## [multiple-choice] What is the difference between liveness and readiness probes?

- [ ] Liveness checks the container, readiness checks the node
- [x] Liveness determines if the container should be restarted; readiness determines if it should receive traffic
- [ ] Liveness runs at startup only, readiness runs continuously
- [ ] There is no functional difference

> **Explanation:** If a liveness probe fails, Kubernetes kills and restarts the container (self-healing). If a readiness probe fails, the pod is removed from service endpoints but keeps running. A pod can be alive but not ready (e.g., warming up a cache).

## [multiple-choice] What happens when a container exceeds its memory limit?

- [ ] It is CPU-throttled
- [ ] A warning is logged
- [x] It is killed (OOMKilled) and restarted
- [ ] The limit is automatically increased

> **Explanation:** Memory is incompressible — you can't "slow down" memory usage. When a container exceeds its memory limit, the kernel kills it with an Out Of Memory (OOM) signal. Kubernetes then restarts it based on the pod's restart policy.

## [short-answer] What kubectl command shows resource consumption of all pods?

    kubectl top pods

> **Explanation:** `kubectl top pods` displays CPU and memory usage for all pods. Requires metrics-server to be installed. Add `--all-namespaces` to see pods in all namespaces, or `-n staging` for a specific namespace.

## [multiple-choice] What does `maxSurge: 1` and `maxUnavailable: 0` mean in a rolling update?

- [x] Create at most 1 extra pod during update; never reduce below the desired count
- [ ] Update 1 pod at a time with possible downtime
- [ ] Allow 1 failed update before aborting
- [ ] Scale up by 1 pod permanently

> **Explanation:** `maxSurge: 1` allows one extra pod above the desired count during the rollout. `maxUnavailable: 0` ensures the full desired count is always available. This is the safest strategy — zero downtime, but slower.

## [multiple-choice] What is the purpose of a Horizontal Pod Autoscaler (HPA)?

- [ ] Automatically increase node count
- [x] Automatically scale pod replicas based on CPU, memory, or custom metrics
- [ ] Distribute pods evenly across nodes
- [ ] Scale the cluster storage

> **Explanation:** HPA watches metrics (CPU utilization, memory, custom metrics) and adjusts the replica count of a Deployment, ReplicaSet, or StatefulSet. It's the standard way to handle varying workloads automatically.
