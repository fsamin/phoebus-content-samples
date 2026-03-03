---
title: "vSphere and OpenStack in Cloud Models"
type: lesson
estimated_duration: "20m"
---

# vSphere and OpenStack in Cloud Models

Now that you understand both the tools (vSphere, OpenStack) and the models (private, public, hybrid), let's see how they fit together in real-world scenarios.

## vSphere: The Private Cloud Workhorse

VMware vSphere is the **dominant platform for enterprise private clouds**. Most Fortune 500 companies run their on-premises infrastructure on vSphere.

### Typical vSphere Private Cloud Architecture

```
┌──────────────────────────────────────────────┐
│              Enterprise Data Center           │
│                                               │
│  ┌─────────────────────────────────────────┐ │
│  │            vCenter Server                │ │
│  └───────────────────┬─────────────────────┘ │
│                      │                        │
│  ┌───────────────────┼─────────────────────┐ │
│  │           vSphere Cluster                │ │
│  │                                          │ │
│  │  ESXi    ESXi    ESXi    ESXi    ESXi   │ │
│  │  Host1   Host2   Host3   Host4   Host5  │ │
│  │  ┌──┐    ┌──┐    ┌──┐    ┌──┐    ┌──┐  │ │
│  │  │VM│    │VM│    │VM│    │VM│    │VM│  │ │
│  │  │VM│    │VM│    │VM│    │VM│    │VM│  │ │
│  │  │VM│    │VM│    │VM│    │VM│    │VM│  │ │
│  │  └──┘    └──┘    └──┘    └──┘    └──┘  │ │
│  └──────────────────────────────────────────┘ │
│                      │                        │
│  ┌───────────────────┴─────────────────────┐ │
│  │         Shared Storage (SAN/vSAN)        │ │
│  └──────────────────────────────────────────┘ │
└──────────────────────────────────────────────┘
```

### vSphere Strengths in Private Cloud

- **Mature and proven** — 25+ years of development
- **Enterprise support** — 24/7 support from Broadcom/VMware
- **Rich ecosystem** — Backup tools (Veeam), monitoring (vROps), automation (Ansible)
- **Stability** — Extremely reliable, battle-tested in production
- **VMware Cloud Foundation** — Full SDDC (Software-Defined Data Center) stack

### vSphere Limitations

- **Expensive licensing** — Per-CPU licensing can cost thousands per host
- **Not self-service** — Admins manage VMs (unless you add vRealize Automation)
- **Vendor lock-in** — VMDK format, proprietary APIs
- **Not cloud-native** — Designed for traditional VM workloads

## OpenStack: The Flexible Cloud Builder

OpenStack can be used to build **private clouds, public clouds, or both**.

### OpenStack as Private Cloud

```
┌──────────────────────────────────────────────┐
│              Enterprise Data Center           │
│                                               │
│  ┌─────────────────────────────────────────┐ │
│  │           OpenStack Services             │ │
│  │  Keystone │ Nova │ Neutron │ Horizon     │ │
│  └───────────────────┬─────────────────────┘ │
│                      │                        │
│  ┌───────────────────┼─────────────────────┐ │
│  │         Compute Nodes (KVM)              │ │
│  │                                          │ │
│  │  Node1    Node2    Node3    Node4       │ │
│  │  ┌──┐    ┌──┐     ┌──┐    ┌──┐         │ │
│  │  │VM│    │VM│     │VM│    │VM│         │ │
│  │  │VM│    │VM│     │VM│    │VM│         │ │
│  │  └──┘    └──┘     └──┘    └──┘         │ │
│  └──────────────────────────────────────────┘ │
│                      │                        │
│  ┌───────────────────┴─────────────────────┐ │
│  │            Ceph Storage Cluster          │ │
│  └──────────────────────────────────────────┘ │
└──────────────────────────────────────────────┘
```

### OpenStack as Public Cloud

Several major cloud providers run OpenStack:

| Provider | Region | Scale |
|----------|--------|-------|
| **OVHcloud** | Europe, worldwide | 400,000+ servers |
| **Infomaniak** | Switzerland | Privacy-focused cloud |
| **City Cloud** | Europe | Multi-region |
| **Vexxhost** | Canada | Public + private cloud |

These providers offer OpenStack APIs, meaning your skills and tools work across all of them — no vendor lock-in.

### OpenStack Strengths

- **No license costs** — Open-source software
- **Self-service by design** — Users provision their own resources
- **API-driven** — Ideal for automation and Infrastructure as Code
- **No vendor lock-in** — Standard APIs, open formats
- **Hyperscale proven** — CERN runs 300,000+ cores on OpenStack

### OpenStack Limitations

- **Complex to deploy** — Many services to install and configure
- **Requires expertise** — You need skilled engineers to operate it
- **Community support** — No single vendor to call at 3 AM (unless you buy a distribution)
- **Upgrades** — Can be challenging with many interdependent services

## Choosing Between Them

| Criteria | Choose vSphere | Choose OpenStack |
|----------|---------------|-----------------|
| **Budget** | License costs acceptable | Minimize licensing costs |
| **Team skills** | VMware certified admins | Linux/Python engineers |
| **Scale** | 10–500 VMs | 100–100,000+ VMs |
| **Users** | IT team provisions VMs | Developers self-service |
| **Automation** | Some automation needed | API-first, full automation |
| **Workloads** | Traditional enterprise apps | Cloud-native apps, DevOps |
| **Support** | Vendor support required | In-house expertise available |

## The Hybrid Approach

Many organizations use **both**:

```
┌──────────────────┐         ┌──────────────────┐
│   vSphere        │         │   Public Cloud   │
│   Private Cloud  │  ◄───►  │   (AWS/Azure)    │
│                  │         │                  │
│  Legacy apps     │         │  New cloud-native│
│  Databases       │         │  applications    │
│  Critical systems│         │  Dev/test envs   │
└──────────────────┘         └──────────────────┘

         or

┌──────────────────┐         ┌──────────────────┐
│   OpenStack      │         │   Public Cloud   │
│   Private Cloud  │  ◄───►  │   (OVHcloud)     │
│                  │         │                  │
│  Production      │         │  Disaster        │
│  workloads       │         │  recovery        │
└──────────────────┘         └──────────────────┘
```

The reality is that most enterprises don't choose just one platform. They use what works best for each workload — and understanding both vSphere and OpenStack gives you the skills to work in any environment.
