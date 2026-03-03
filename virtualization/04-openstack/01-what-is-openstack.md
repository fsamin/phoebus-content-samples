---
title: "What is OpenStack?"
type: lesson
estimated_duration: "20m"
---

# What is OpenStack?

## The Big Picture

**OpenStack** is an open-source software platform for building and managing **cloud infrastructure**. It turns a collection of physical servers, storage, and networking into a self-service cloud — similar to AWS or Azure, but running in your own data center.

While VMware vSphere is a virtualization management platform that you operate manually (or semi-automatically), OpenStack is designed to be a **cloud platform**: it provides APIs, self-service portals, and automation from the ground up.

```
┌─────────────────────────────────────────────┐
│              OpenStack                       │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐    │
│  │ Compute  │ │ Storage  │ │ Network  │    │
│  │ (Nova)   │ │ (Cinder) │ │(Neutron) │    │
│  └──────────┘ └──────────┘ └──────────┘    │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐    │
│  │ Identity │ │  Images  │ │Dashboard │    │
│  │(Keystone)│ │ (Glance) │ │(Horizon) │    │
│  └──────────┘ └──────────┘ └──────────┘    │
├─────────────────────────────────────────────┤
│        Hypervisor (KVM, usually)             │
├─────────────────────────────────────────────┤
│        Physical Servers                      │
└─────────────────────────────────────────────┘
```

## Origin

OpenStack was born in 2010 as a joint project between:
- **NASA** — who had built a cloud platform called Nebula
- **Rackspace** — a hosting company who had built cloud storage software

They open-sourced their work, and it quickly grew into the largest open-source cloud project in the world, with contributions from hundreds of companies including Red Hat, Canonical, SUSE, Huawei, and many others.

## OpenStack vs vSphere

| Aspect | VMware vSphere | OpenStack |
|--------|---------------|-----------|
| **License** | Commercial (expensive) | Open-source (free) |
| **Approach** | Admin-managed infrastructure | Self-service cloud |
| **Hypervisor** | ESXi (proprietary) | KVM (default), also supports ESXi, Xen |
| **User interface** | vSphere Client (admin only) | Horizon dashboard (for all users) |
| **API** | Proprietary | RESTful, AWS-compatible (optional) |
| **Provisioning** | Manual or semi-automated | Fully automated, API-driven |
| **Complexity** | Moderate | High (many components) |
| **Support** | VMware/Broadcom | Community + vendors (Red Hat, Canonical, etc.) |

## Key Philosophy Differences

### vSphere: The Admin Model
In vSphere, system administrators create and manage VMs on behalf of users. Users typically submit a request ("I need a server with 4 CPUs and 16 GB RAM"), and an admin provisions it.

### OpenStack: The Self-Service Model
In OpenStack, users log into a portal (Horizon) or use the API/CLI, and create VMs themselves — instantly. They choose the size, the OS image, and the network. No admin intervention needed.

```
vSphere model:
  User → Request → Admin → Create VM → Deliver to user
  (days/weeks)

OpenStack model:
  User → Click "Launch Instance" → VM ready
  (seconds/minutes)
```

This self-service model is what makes OpenStack a **cloud platform** rather than just a virtualization management tool.

## Who Uses OpenStack?

OpenStack is used by organizations of all sizes:

- **Telecom companies** — AT&T, China Mobile, Deutsche Telekom
- **Research institutions** — CERN, NASA
- **Cloud providers** — OVHcloud, Infomaniak, City Cloud
- **Enterprises** — Walmart, Bloomberg, eBay
- **Government** — Various government agencies worldwide

It powers some of the largest private and public clouds in the world.
