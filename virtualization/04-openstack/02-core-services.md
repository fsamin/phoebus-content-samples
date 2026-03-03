---
title: "OpenStack Core Services"
type: lesson
estimated_duration: "25m"
---

# OpenStack Core Services

OpenStack is not a monolithic application — it is made up of **independent services** that communicate via APIs. Each service handles one aspect of the cloud. You can think of it like a set of Lego blocks that you assemble together.

## The Core Services

### Keystone — Identity Service

**Keystone** is the front door. It handles **authentication** (who are you?) and **authorization** (what are you allowed to do?).

Every request to any OpenStack service goes through Keystone first:

```
User → Keystone → "Here's your token" → Use token to call Nova, Neutron, etc.
```

Keystone manages:
- **Users** and **groups**
- **Projects** (isolated environments, like AWS accounts)
- **Roles** (admin, member, reader)
- **Service catalog** (list of all available services and their URLs)

### Nova — Compute Service

**Nova** is the heart of OpenStack. It manages **virtual machines** (called **instances** in OpenStack terminology).

What Nova does:
- Receives "launch an instance" requests
- Decides which physical host should run the instance (scheduling)
- Communicates with the hypervisor (KVM) to create the VM
- Manages the instance lifecycle (start, stop, resize, delete)

```
User: "Launch an Ubuntu instance with 4 CPUs and 8 GB RAM"
  │
  ▼
Nova Scheduler: "Host 7 has the most free resources, put it there"
  │
  ▼
Nova Compute (on Host 7): Creates the VM via KVM
```

### Neutron — Networking Service

**Neutron** manages all the virtual networking:

- **Networks** — Virtual Layer 2 networks
- **Subnets** — IP address ranges
- **Routers** — Connect networks together and to the outside world
- **Floating IPs** — Public IP addresses you can attach to instances
- **Security Groups** — Firewall rules (which ports are open)

```
┌──────────────────────────────┐
│       Virtual Router         │
│    (connects to internet)    │
└─────────┬────────────────────┘
          │
┌─────────┴────────────────────┐
│     Private Network          │
│     10.0.0.0/24              │
│                              │
│  ┌──────┐  ┌──────┐         │
│  │ VM-1 │  │ VM-2 │         │
│  │.10   │  │.11   │         │
│  └──────┘  └──────┘         │
└──────────────────────────────┘
```

Each user/project gets their own isolated networks. VM-1 in Project A cannot see or reach VM-1 in Project B unless explicitly allowed.

### Glance — Image Service

**Glance** stores and manages **OS images** — the templates used to boot instances.

Common images:
- Ubuntu 22.04 Cloud Image
- CentOS Stream 9
- Debian 12
- Windows Server 2022

When you launch an instance, Nova asks Glance for the image, copies it to the compute host, and boots the VM from it.

### Cinder — Block Storage Service

**Cinder** provides persistent **block storage volumes** — virtual hard drives that you can attach to instances.

Key features:
- Volumes **persist** even after an instance is deleted
- You can **detach** a volume from one instance and **attach** it to another
- Supports **snapshots** for backup
- Works with many storage backends (Ceph, NetApp, Pure Storage, local LVM)

```
Instance "web-01"
  └── /dev/vdb ← Cinder Volume "data-disk" (100 GB)

# Delete instance "web-01"
# Volume "data-disk" still exists!

Instance "web-02"
  └── /dev/vdb ← Attach the same volume
```

### Horizon — Dashboard

**Horizon** is the web-based user interface. It provides a visual way to interact with all OpenStack services:

- Launch and manage instances
- Create networks and routers
- Upload images
- Manage volumes
- View usage and quotas

It is optional — everything Horizon does can also be done via the **CLI** (`openstack` command) or the **REST API**.

## How They Work Together

Here's what happens when a user launches a new instance:

```
1. User authenticates with Keystone → gets a token
2. User sends "launch instance" to Nova with the token
3. Nova asks Glance for the OS image
4. Nova asks Neutron to set up networking (port, IP address)
5. Nova Scheduler picks the best host
6. Nova Compute (on chosen host) creates the VM via KVM
7. If requested, Cinder creates and attaches a volume
8. Instance is ready — user gets the IP address
```

All of this happens **automatically** in seconds. No human intervention required.

## Optional Services

Beyond the core, OpenStack offers many additional services:

| Service | Name | Purpose |
|---------|------|---------|
| **Swift** | Object Storage | S3-like object storage |
| **Heat** | Orchestration | Infrastructure as Code (like CloudFormation) |
| **Octavia** | Load Balancer | Distribute traffic across instances |
| **Barbican** | Key Manager | Secrets and certificate management |
| **Ironic** | Bare Metal | Provision physical servers (not just VMs) |
| **Magnum** | Container Orchestration | Deploy Kubernetes clusters |
