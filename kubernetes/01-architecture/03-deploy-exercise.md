---
title: "Deploy Your First Application"
type: terminal-exercise
estimated_duration: "15m"
---

# Deploy Your First Application

You have a Kubernetes cluster ready. Let's deploy a web application, expose it, and verify it's running.

## Step 1: Create a deployment

Deploy an nginx web server with 2 replicas.

```console
$ ▌
```

- [x] `kubectl create deployment web --image=nginx:1.25-alpine --replicas=2` — Creates a deployment named "web" with 2 nginx pods.
- [ ] `kubectl run web --image=nginx:1.25-alpine` — Creates a single pod, not a managed deployment with replicas.
- [ ] `docker run -d nginx:1.25-alpine` — This is a Docker command, not Kubernetes.

```output
deployment.apps/web created
```

## Step 2: Verify pods are running

Check that both replicas are up and ready.

```console
$ ▌
```

- [ ] `kubectl get deployments` — Shows the deployment but not individual pod status.
- [x] `kubectl get pods -l app=web` — Lists pods with the label app=web, showing status of each replica.
- [ ] `docker ps` — Docker command, doesn't show Kubernetes pods.

```output
NAME                   READY   STATUS    RESTARTS   AGE
web-5d94cf4b7c-k2j8m   1/1     Running   0          15s
web-5d94cf4b7c-x9p4n   1/1     Running   0          15s
```

## Step 3: Expose the deployment as a service

Create a Service so the deployment is reachable within the cluster.

```console
$ ▌
```

- [ ] `kubectl port-forward deployment/web 8080:80` — Creates a temporary tunnel, not a persistent service.
- [x] `kubectl expose deployment web --port=80 --target-port=80 --type=ClusterIP` — Creates a ClusterIP service routing traffic to the web pods.
- [ ] `kubectl create service web --port=80` — Invalid syntax.

```output
service/web exposed
```

## Step 4: Test connectivity

From within the cluster, verify the service responds.

```console
$ ▌
```

- [ ] `curl http://web` — Only works from inside a pod, not from the kubectl host.
- [x] `kubectl run test --rm -it --image=alpine/curl -- curl -s http://web` — Runs a temporary pod to test the service from inside the cluster.
- [ ] `ping web` — Ping doesn't test HTTP connectivity and doesn't work from outside the cluster.

```output
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
...
```
