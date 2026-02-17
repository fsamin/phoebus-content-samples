---
title: "Helm Quiz"
type: quiz
estimated_duration: "10m"
---

# Helm Quiz

## [multiple-choice] What is a Helm chart?

- [ ] A Kubernetes monitoring dashboard
- [x] A package of Kubernetes manifest templates with configurable values
- [ ] A Docker image format
- [ ] A CI/CD pipeline definition

> **Explanation:** A Helm chart bundles Kubernetes YAML templates with default values, metadata, and optional dependencies into a reusable, versionable package. It's the standard unit of deployment for Kubernetes applications.

## [multiple-choice] What command previews rendered Kubernetes manifests without deploying?

- [ ] `helm install --preview`
- [x] `helm template myrelease ./myapp`
- [ ] `helm show manifests ./myapp`
- [ ] `helm render ./myapp`

> **Explanation:** `helm template` renders the chart templates locally with the provided values and outputs the resulting Kubernetes YAML. It doesn't require a cluster connection and is perfect for debugging templates.

## [short-answer] How do you access a value defined as `image.tag` in values.yaml within a Helm template?

    .Values.image.tag

> **Explanation:** Helm templates use the `.Values` object to access values from `values.yaml`. Nested YAML keys become nested properties: `image.tag: "1.0"` is accessed as `{{ .Values.image.tag }}`.

## [multiple-choice] How do you deploy the same chart with different configurations for dev and prod?

- [ ] Create two separate charts
- [ ] Modify values.yaml for each environment
- [x] Use environment-specific values files: `helm install -f values-prod.yaml`
- [ ] Use different Helm versions

> **Explanation:** The `-f` flag specifies a values file that overrides the chart's defaults. Maintain `values-dev.yaml`, `values-staging.yaml`, and `values-prod.yaml` alongside your chart for environment-specific configuration.

## [multiple-choice] What does `helm rollback myrelease 2` do?

- [ ] Rolls back to Helm version 2
- [ ] Deletes revision 2
- [x] Reverts the release to its configuration at revision 2
- [ ] Rolls back the last 2 deployments

> **Explanation:** Helm tracks every deployment as a numbered revision. `helm rollback myrelease 2` deploys the exact state of revision 2 — same image, same config, same replicas. This is a key advantage over managing raw Kubernetes manifests.
