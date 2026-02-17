---
title: "Templates & Values"
type: lesson
estimated_duration: "20m"
---

# Templates & Values

## Go Templates in Helm

Helm uses Go's `text/template` engine. Templates access values through the `.Values` object.

### Basic Syntax

```yaml
# templates/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-{{ .Chart.Name }}
  labels:
    app: {{ .Chart.Name }}
    release: {{ .Release.Name }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Chart.Name }}
  template:
    metadata:
      labels:
        app: {{ .Chart.Name }}
    spec:
      containers:
        - name: {{ .Chart.Name }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          ports:
            - containerPort: {{ .Values.service.port }}
          resources:
            {{- toYaml .Values.resources | nindent 12 }}
```

### Default values.yaml

```yaml
# values.yaml
replicaCount: 1

image:
  repository: mycompany/myapp
  tag: "1.0.0"
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 8080

resources:
  limits:
    cpu: 500m
    memory: 256Mi
  requests:
    cpu: 100m
    memory: 128Mi

ingress:
  enabled: false
  host: ""
```

## Template Functions

### Conditionals

```yaml
{{- if .Values.ingress.enabled }}
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: {{ .Release.Name }}-ingress
spec:
  rules:
    - host: {{ .Values.ingress.host }}
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: {{ .Release.Name }}-svc
                port:
                  number: {{ .Values.service.port }}
{{- end }}
```

### Loops

```yaml
{{- range .Values.env }}
- name: {{ .name }}
  value: {{ .value | quote }}
{{- end }}
```

### Helper Templates (_helpers.tpl)

```yaml
# templates/_helpers.tpl
{{- define "myapp.fullname" -}}
{{ .Release.Name }}-{{ .Chart.Name }}
{{- end }}

{{- define "myapp.labels" -}}
app.kubernetes.io/name: {{ .Chart.Name }}
app.kubernetes.io/instance: {{ .Release.Name }}
app.kubernetes.io/version: {{ .Chart.AppVersion }}
{{- end }}
```

Usage:

```yaml
metadata:
  name: {{ include "myapp.fullname" . }}
  labels:
    {{- include "myapp.labels" . | nindent 4 }}
```

## Environment-specific Values

```yaml
# values-dev.yaml
replicaCount: 1
image:
  tag: "latest"
resources:
  limits:
    cpu: 200m
    memory: 128Mi
```

```yaml
# values-prod.yaml
replicaCount: 3
image:
  tag: "1.0.0"
resources:
  limits:
    cpu: "1"
    memory: 512Mi
ingress:
  enabled: true
  host: app.company.com
```

```bash
helm install myapp ./myapp -f values-dev.yaml   # Dev
helm install myapp ./myapp -f values-prod.yaml   # Production
```

## Debugging Templates

```bash
# Render templates locally (no cluster needed)
helm template myrelease ./myapp -f values-prod.yaml

# Dry-run with server-side validation
helm install myrelease ./myapp --dry-run --debug

# Lint the chart
helm lint ./myapp
```

## Summary

Templates + values = dynamic Kubernetes manifests. Write templates once, deploy everywhere with different values files. Use `helm template` and `helm lint` to validate before deploying.
