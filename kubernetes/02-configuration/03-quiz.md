---
title: "Configuration Quiz"
type: quiz
estimated_duration: "10m"
---

# Kubernetes Configuration Quiz

## [multiple-choice] What is the difference between ConfigMaps and Secrets?

- [ ] ConfigMaps are YAML, Secrets are JSON
- [ ] ConfigMaps are immutable, Secrets are mutable
- [x] ConfigMaps store non-sensitive data in plain text; Secrets store sensitive data base64-encoded
- [ ] ConfigMaps are cluster-wide, Secrets are namespace-scoped

> **Explanation:** Both are namespace-scoped and structured similarly. The key difference is that Secrets are base64-encoded (not encrypted by default!) and subject to stricter RBAC policies. Use ConfigMaps for app config, Secrets for passwords and keys.

## [multiple-choice] How can a pod consume a ConfigMap value?

- [ ] Only as environment variables
- [ ] Only as mounted files
- [x] As environment variables OR as mounted files
- [ ] Only through the Kubernetes API

> **Explanation:** ConfigMap values can be injected as environment variables (`envFrom` or individual `env` entries) or mounted as files in a volume. Choose based on how your application reads config.

## [short-answer] What command creates a Kubernetes Secret from literal values?

    kubectl create secret generic my-secret --from-literal=key=value

> **Explanation:** `kubectl create secret generic` creates an Opaque secret. `--from-literal` specifies key-value pairs. You can also use `--from-file` to create secrets from files.

## [multiple-choice] What happens to data on a PersistentVolume with reclaim policy "Retain" when the PVC is deleted?

- [ ] The data is immediately deleted
- [ ] The PV is recycled for new claims
- [x] The PV and data are preserved; an admin must manually clean up
- [ ] The data is backed up automatically

> **Explanation:** "Retain" means the PV stays in a "Released" state after PVC deletion. The data is preserved but the PV can't be reused until an admin manually clears it. This is the safest policy for important data.

## [multiple-choice] What is dynamic volume provisioning?

- [ ] Volumes that resize automatically
- [x] PersistentVolumes are created automatically when a PVC is submitted, based on a StorageClass
- [ ] Volumes that are cached in memory
- [ ] Storage allocated during pod startup

> **Explanation:** Dynamic provisioning uses a `StorageClass` to automatically create PVs when PVCs are submitted. This eliminates the need for admins to pre-provision storage, and is the standard approach in cloud environments.
