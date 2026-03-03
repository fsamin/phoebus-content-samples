---
title: "Private, Public, and Hybrid Cloud"
type: lesson
estimated_duration: "25m"
---

# Private, Public, and Hybrid Cloud

## What is "The Cloud"?

The word "cloud" is often used loosely, but it has a specific definition from the **NIST** (National Institute of Standards and Technology):

> Cloud computing is a model for enabling on-demand network access to a shared pool of configurable computing resources that can be rapidly provisioned and released with minimal management effort.

In simpler terms: **cloud = computing resources available on-demand, pay-as-you-go, via a network.**

The key characteristics are:

1. **On-demand self-service** — Get resources when you need them, without calling anyone
2. **Broad network access** — Available from anywhere via the internet
3. **Resource pooling** — Resources shared across many users (multi-tenancy)
4. **Rapid elasticity** — Scale up or down quickly
5. **Measured service** — Pay only for what you use

## Private Cloud

A **private cloud** is cloud infrastructure operated **solely for one organization**. The hardware is either:
- Owned by the organization and located in their data center, or
- Dedicated hardware at a hosting provider

```
┌─────────────────────────────────────────┐
│          Company's Data Center           │
│                                          │
│    ┌──────────────────────────┐          │
│    │    Private Cloud          │          │
│    │  (vSphere or OpenStack)   │          │
│    │                          │          │
│    │  ┌────┐ ┌────┐ ┌────┐   │          │
│    │  │VM-1│ │VM-2│ │VM-3│   │          │
│    │  └────┘ └────┘ └────┘   │          │
│    └──────────────────────────┘          │
│                                          │
│    Only accessible by company employees  │
└─────────────────────────────────────────┘
```

### Technologies Used

| Technology | Type | Best For |
|-----------|------|----------|
| **VMware vSphere** | Commercial | Enterprises with VMware expertise |
| **OpenStack** | Open-source | Organizations wanting flexibility and no license costs |
| **Proxmox VE** | Open-source | Small to medium private clouds |
| **Nutanix** | Commercial | Hyper-converged infrastructure |

### Advantages

- **Full control** — You own the hardware and data
- **Security** — Data never leaves your premises
- **Compliance** — Easier to meet regulatory requirements (GDPR, HIPAA, etc.)
- **Predictable costs** — No surprise bills (you own the hardware)

### Disadvantages

- **High upfront cost** — Buying servers, storage, networking, and a data center
- **Capacity planning** — You must predict future needs and buy hardware in advance
- **Maintenance** — You manage everything: hardware failures, software updates, security
- **Limited scalability** — Scaling up means buying more hardware (weeks/months)

## Public Cloud

A **public cloud** is cloud infrastructure operated by a **third-party provider** and shared among multiple organizations (tenants). You rent resources on-demand.

```
┌─────────────────────────────────────────┐
│        Cloud Provider Data Center        │
│                                          │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐│
│  │Company A │ │Company B │ │Company C ││
│  │ ┌──┐┌──┐│ │ ┌──┐     │ │ ┌──┐┌──┐ ││
│  │ │VM││VM││ │ │VM│     │ │ │VM││VM│ ││
│  │ └──┘└──┘│ │ └──┘     │ │ └──┘└──┘ ││
│  └──────────┘ └──────────┘ └──────────┘│
│                                          │
│  Same physical infrastructure, isolated  │
└─────────────────────────────────────────┘
```

### Major Public Cloud Providers

| Provider | Cloud | Virtualization Technology |
|----------|-------|--------------------------|
| **Amazon** | AWS (EC2) | Nitro (custom KVM) |
| **Microsoft** | Azure | Hyper-V |
| **Google** | Google Cloud | KVM |
| **OVHcloud** | OVHcloud | OpenStack + KVM |
| **Alibaba** | Alibaba Cloud | KVM |

### Advantages

- **No upfront cost** — Pay only for what you use (per hour/minute)
- **Infinite scalability** — Need 1000 servers? Click a button
- **No maintenance** — The provider handles hardware, cooling, security
- **Global reach** — Data centers worldwide
- **Innovation** — Access to managed services (databases, AI, etc.)

### Disadvantages

- **Less control** — You're on the provider's platform, with their rules
- **Unpredictable costs** — Bills can spike unexpectedly
- **Vendor lock-in** — Moving away can be difficult and expensive
- **Data sovereignty** — Your data is in someone else's data center
- **Shared infrastructure** — Security depends on the provider's isolation

## Hybrid Cloud

A **hybrid cloud** combines private and public cloud, with workloads able to **move between them**.

```
┌──────────────────┐         ┌──────────────────┐
│   Private Cloud   │  ◄───► │   Public Cloud    │
│   (On-premises)   │  VPN   │   (AWS/Azure/GCP) │
│                   │   or   │                   │
│  Critical apps    │ Direct │  Burst workloads  │
│  Sensitive data   │ Connect│  Dev/test          │
│  Compliance       │        │  Global services   │
└──────────────────┘         └──────────────────┘
```

### Use Cases

1. **Cloud bursting** — Run baseline on private cloud, burst to public cloud during peak demand
2. **Data sovereignty** — Keep sensitive data on-premises, run web frontends in public cloud
3. **Disaster recovery** — Replicate private cloud to public cloud as a backup
4. **Development** — Develop and test in public cloud, deploy to production on private cloud

### Connection Methods

| Method | Description |
|--------|-------------|
| **VPN** | Encrypted tunnel over the internet |
| **Direct Connect / ExpressRoute** | Dedicated physical link to the cloud provider |
| **Federation** | Same identity system across both clouds |

## Service Models: IaaS, PaaS, SaaS

Cloud services are categorized by **how much the provider manages** for you:

```
                You manage ↑        Provider manages ↑
                           │                          │
┌──────────┐    ┌──────────┤    ┌──────────┐    ┌─────┤
│   IaaS   │    │ App      │    │   PaaS   │    │     │
│          │    │ Data     │    │          │    │     │
│ You get: │    │ Runtime  │    │ You get: │    │ SaaS│
│ VMs,     │    │ OS       │    │ Platform │    │     │
│ network, │    ├──────────┤    │ to deploy│    │You  │
│ storage  │    │ Infra    │    │ your code│    │just │
│          │    │ (managed)│    │          │    │use  │
│          │    │          │    ├──────────┤    │the  │
│          │    │          │    │ Infra    │    │app  │
│          │    │          │    │ (managed)│    │     │
└──────────┘    └──────────┘    └──────────┘    └─────┘
```

| Model | What You Get | Examples |
|-------|-------------|---------|
| **IaaS** | Virtual machines, networks, storage | AWS EC2, OpenStack, Azure VMs |
| **PaaS** | Platform to run your code, no server management | Heroku, Google App Engine, Cloud Foundry |
| **SaaS** | Ready-to-use application | Gmail, Slack, Salesforce |

**vSphere and OpenStack are IaaS solutions** — they provide the infrastructure layer (VMs, networking, storage) on which you build everything else.
