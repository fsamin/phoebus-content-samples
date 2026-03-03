---
title: "vSphere Architecture"
type: lesson
estimated_duration: "25m"
---

# vSphere Architecture

## What is vSphere?

**VMware vSphere** is not a single product — it's a **suite** of products that work together to virtualize an entire data center. Think of it as a complete ecosystem for running and managing virtual machines at scale.

The two main components are:

1. **ESXi** — The Type 1 hypervisor installed on each physical server
2. **vCenter Server** — The centralized management platform

## ESXi: The Hypervisor

**ESXi** (Elastic Sky X integrated) is a tiny, specialized operating system whose sole purpose is to run virtual machines.

Key facts:
- Its installation footprint is only **~150 MB**
- It has no general-purpose OS — no unnecessary services, no desktop
- It is managed remotely via a web interface or command line (SSH)
- Each physical server in your data center runs ESXi

When you install ESXi on a server, you get a **host**. Each host can run dozens or even hundreds of VMs depending on its hardware.

```
Physical Server
┌─────────────────────────────────┐
│          VMware ESXi            │
│  ┌───────┐ ┌───────┐ ┌───────┐ │
│  │ VM-1  │ │ VM-2  │ │ VM-3  │ │
│  │Ubuntu │ │Windows│ │CentOS │ │
│  └───────┘ └───────┘ └───────┘ │
└─────────────────────────────────┘
```

## vCenter Server: The Brain

While ESXi manages a single host, **vCenter Server** manages **all your hosts** from one place. It is the centralized control plane of vSphere.

```
                  ┌──────────────┐
                  │   vCenter    │
                  │   Server     │
                  └──────┬───────┘
              ┌──────────┼──────────┐
              ▼          ▼          ▼
         ┌────────┐ ┌────────┐ ┌────────┐
         │ ESXi   │ │ ESXi   │ │ ESXi   │
         │ Host 1 │ │ Host 2 │ │ Host 3 │
         │ 20 VMs │ │ 15 VMs │ │ 25 VMs │
         └────────┘ └────────┘ └────────┘
```

With vCenter, you can:

- **See all VMs** across all hosts in a single dashboard
- **vMotion** — Live-migrate VMs between hosts
- **DRS** (Distributed Resource Scheduler) — Automatically balance VM load across hosts
- **HA** (High Availability) — Restart VMs on surviving hosts after a failure
- **Templates** — Create and manage VM templates centrally
- **Permissions** — Control who can do what, on which VMs

## Clusters

A **cluster** is a group of ESXi hosts managed together by vCenter. Clusters enable the most powerful vSphere features:

```
Cluster "Production"
┌─────────────────────────────────────┐
│  Host 1      Host 2      Host 3    │
│  ┌──────┐    ┌──────┐    ┌──────┐  │
│  │ VMs  │◄──►│ VMs  │◄──►│ VMs  │  │
│  └──────┘    └──────┘    └──────┘  │
│                                     │
│  Features enabled:                  │
│  ✓ vMotion (live migration)         │
│  ✓ DRS (automatic load balancing)   │
│  ✓ HA (automatic failover)          │
└─────────────────────────────────────┘
```

When DRS is enabled, vCenter continuously monitors resource usage. If Host 1 is overloaded and Host 3 has spare capacity, DRS will automatically migrate VMs from Host 1 to Host 3.

## Shared Storage

For features like vMotion and HA to work, all hosts in a cluster need access to the **same storage**. VMs' virtual disks must be on shared storage so that any host can access them.

Common shared storage technologies:

| Technology | Description |
|-----------|-------------|
| **SAN** (Storage Area Network) | Dedicated storage network using Fibre Channel or iSCSI |
| **NFS** | Network File System — file-level shared storage |
| **vSAN** | VMware's software-defined storage using local disks in each host |

```
         Host 1       Host 2       Host 3
            │            │            │
            └────────────┼────────────┘
                         │
                  ┌──────┴──────┐
                  │   Shared    │
                  │   Storage   │
                  │  (SAN/NFS)  │
                  └─────────────┘
```

## The vSphere Client

vSphere is managed through a **web-based interface** called the **vSphere Client**. It connects to vCenter Server and provides:

- A visual inventory of all data centers, clusters, hosts, and VMs
- Performance monitoring graphs
- Task and alarm management
- Configuration wizards

There is no need to install any software on your workstation — just open a web browser and connect to `https://vcenter.example.com`.
