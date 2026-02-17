---
title: "Introduction to Helm"
type: lesson
estimated_duration: "20m"
---

# Introduction to Helm

## What is Helm?

Helm is the **package manager for Kubernetes**. It packages Kubernetes manifests into reusable, versionable, and configurable units called **charts**.

Think of it as `apt` or `brew` for Kubernetes:
- **Without Helm**: maintain dozens of YAML files per application
- **With Helm**: one chart, different values per environment

## Key Concepts

| Concept | Description |
|---------|-------------|
| **Chart** | A package of Kubernetes templates + defaults |
| **Release** | A deployed instance of a chart |
| **Values** | Configuration that customizes a chart |
| **Repository** | A collection of charts (like Docker Hub for images) |
| **Template** | Kubernetes YAML with Go template placeholders |

## Chart Structure

```
mychart/
├── Chart.yaml          # Chart metadata (name, version, description)
├── values.yaml         # Default configuration values
├── charts/             # Dependencies (sub-charts)
├── templates/          # Kubernetes manifest templates
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── configmap.yaml
│   ├── _helpers.tpl    # Reusable template functions
│   ├── NOTES.txt       # Post-install instructions
│   └── tests/
│       └── test-connection.yaml
└── .helmignore         # Files to exclude from packaging
```

## Chart.yaml

```yaml
apiVersion: v2
name: myapp
description: A web application with PostgreSQL backend
type: application
version: 0.1.0        # Chart version
appVersion: "1.0.0"   # Application version
dependencies:
  - name: postgresql
    version: "15.x"
    repository: "https://charts.bitnami.com/bitnami"
```

## Essential Commands

```bash
# Create a new chart
helm create myapp

# Install a chart
helm install myrelease ./myapp
helm install myrelease ./myapp -f prod-values.yaml

# List releases
helm list

# Upgrade a release
helm upgrade myrelease ./myapp -f prod-values.yaml

# Rollback to a previous version
helm rollback myrelease 1

# Uninstall a release
helm uninstall myrelease

# Preview what would be deployed
helm template myrelease ./myapp
helm install myrelease ./myapp --dry-run --debug

# Add a chart repository
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm search repo postgres
```

## Why Helm?

1. **Reproducibility** — same chart, same values = same deployment
2. **Environment management** — different values files for dev/staging/prod
3. **Rollbacks** — `helm rollback` reverts to any previous release
4. **Dependency management** — embed PostgreSQL, Redis as sub-charts
5. **Community charts** — install Prometheus, Grafana, nginx-ingress in one command

## Summary

Helm abstracts Kubernetes complexity into reusable, configurable packages. Master Helm and you'll deploy applications consistently across any number of environments and clusters.
