---
title: "Key Hypervisor Features"
type: lesson
estimated_duration: "20m"
---

# Key Hypervisor Features

All modern hypervisors — whether Type 1 or Type 2 — provide a set of essential features that make virtualization practical and powerful. Understanding these features is crucial before diving into specific products.

## Snapshots

A **snapshot** captures the entire state of a VM at a specific moment: its disk, memory, and configuration.

Think of it as a **save point in a video game**. You can:

1. Take a snapshot before a risky operation (OS upgrade, configuration change)
2. Perform the operation
3. If something goes wrong, **revert** to the snapshot instantly

```
            Snapshot A          Snapshot B
                │                   │
────────────────┼───────────────────┼──────────→ time
                │                   │
          "Before upgrade"    "After upgrade"
                │
                └── Revert here if upgrade fails
```

> ⚠️ **Warning:** Snapshots are not backups. They depend on the original disk file. If the underlying storage fails, snapshots are lost too.

## Live Migration

**Live migration** (called **vMotion** in VMware) moves a running VM from one physical host to another **without downtime**.

```
   Host A                    Host B
┌──────────┐             ┌──────────┐
│   VM-X   │  ────────→  │   VM-X   │
│ (running) │   migrate   │ (running) │
└──────────┘             └──────────┘
```

How it works:

1. VM memory is copied to the destination host while the VM keeps running
2. The remaining "dirty" (changed) memory pages are re-copied
3. The VM is briefly paused (milliseconds), final memory is transferred
4. The VM resumes on the new host

This allows **hardware maintenance without service interruption**: you migrate all VMs off a server, replace a faulty component, and migrate them back.

## Resource Pools and Limits

Hypervisors let you control how much hardware each VM can use:

| Setting | Description |
|---------|-------------|
| **Reservation** | Guaranteed minimum resources (e.g., "this VM always gets at least 4 GB RAM") |
| **Limit** | Maximum resources (e.g., "this VM can never use more than 8 GB RAM") |
| **Shares** | Priority when resources are scarce (e.g., "production VMs get priority over dev VMs") |

This prevents a single VM from monopolizing the host's resources.

## High Availability (HA)

**High Availability** automatically restarts VMs on another host if their current host fails.

```
   Host A (fails!)           Host B
┌──────────┐              ┌──────────┐
│   VM-X   │  ──restart──→│   VM-X   │
│   VM-Y   │              │   VM-Y   │
└──────────┘              └──────────┘
      ✗                        ✓
```

Unlike live migration, HA involves a brief **downtime** (the VM reboots). But it ensures that hardware failure doesn't take your services offline permanently.

## Templates and Cloning

Instead of installing an OS from scratch every time, you can:

1. **Create a template** — A pre-configured VM image (OS installed, software configured, security hardened)
2. **Clone** the template to create new VMs in seconds

```
Template "Ubuntu 22.04 Base"
    │
    ├── Clone → web-server-01
    ├── Clone → web-server-02
    └── Clone → web-server-03
```

This is the foundation of **Infrastructure as Code**: instead of manually setting up servers, you define a template and stamp out identical copies.

## Virtual Networking

Modern hypervisors include sophisticated virtual networking:

- **Virtual switches** — Connect VMs together
- **VLANs** — Isolate network traffic between groups of VMs
- **Distributed switches** — Span virtual networking across multiple hosts
- **Firewalls** — Filter traffic between VMs (micro-segmentation)

This allows building complex network topologies entirely in software, without physical cables or switches.

## Storage Features

| Feature | Description |
|---------|-------------|
| **Thin provisioning** | Disks grow on-demand instead of pre-allocating all space |
| **Storage migration** | Move VM disks between storage systems without downtime |
| **Deduplication** | Eliminate duplicate data blocks across VMs |
| **Encryption** | Encrypt VM disk files at rest |
