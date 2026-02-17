---
title: "Fix the Deployment Health Check"
type: code-exercise
mode: choose-the-fix
estimated_duration: "10m"
target:
  file: "deployment.yaml"
  lines: [25, 26, 27, 28, 29]
---

# Fix the Deployment Health Check

The API deployment keeps restarting every few minutes. Pods show `CrashLoopBackOff` in the events. Looking at the deployment manifest, you notice the liveness probe configuration seems wrong.

The application takes about 30 seconds to start and the health endpoint is at `/healthz` on port 8080.

## Patches

### [x] Fix initialDelaySeconds and add a startup probe

The liveness probe fires too early (5s) before the app is ready (30s startup), causing Kubernetes to kill it repeatedly. Adding a startup probe and increasing the initial delay fixes the crash loop.

```diff
--- a/deployment.yaml
+++ b/deployment.yaml
@@ -22,8 +22,16 @@
           ports:
             - containerPort: 8080
+          startupProbe:
+            httpGet:
+              path: /healthz
+              port: 8080
+            failureThreshold: 10
+            periodSeconds: 5
           livenessProbe:
             httpGet:
               path: /healthz
               port: 8080
-            initialDelaySeconds: 5
+            initialDelaySeconds: 0
-            periodSeconds: 5
+            periodSeconds: 10
-            failureThreshold: 1
+            failureThreshold: 3
```

### [ ] Just increase the initialDelaySeconds

Increasing the delay helps but doesn't solve the fundamental issue. Without a startup probe, slow starts (CI, cold storage) can still trigger kills. And `failureThreshold: 1` is too aggressive.

```diff
--- a/deployment.yaml
+++ b/deployment.yaml
@@ -27,3 +27,3 @@
               port: 8080
-            initialDelaySeconds: 5
+            initialDelaySeconds: 60
             periodSeconds: 5
```

### [ ] Remove the liveness probe entirely

Removing the probe prevents restarts but also removes self-healing. If the app deadlocks, it will never be restarted.

```diff
--- a/deployment.yaml
+++ b/deployment.yaml
@@ -24,9 +24,3 @@
             - containerPort: 8080
-          livenessProbe:
-            httpGet:
-              path: /healthz
-              port: 8080
-            initialDelaySeconds: 5
-            periodSeconds: 5
-            failureThreshold: 1
```
