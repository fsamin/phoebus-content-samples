---
title: "Type 1 vs Type 2 Hypervisors"
type: lesson
estimated_duration: "20m"
---

# Type 1 vs Type 2 Hypervisors

Not all hypervisors are created equal. They are divided into two categories based on **where they run** relative to the hardware.

## Type 2: The "Hosted" Hypervisor

A Type 2 hypervisor runs **on top of a regular operating system**, just like any other application.

```
┌─────────┐ ┌─────────┐
│  VM-A   │ │  VM-B   │
└────┬────┘ └────┬────┘
┌────┴───────────┴────┐
│  Type 2 Hypervisor  │  ← an application
├─────────────────────┤
│   Host OS (Windows, │
│   macOS, Linux)     │
├─────────────────────┤
│   Physical Hardware │
└─────────────────────┘
```

### Examples

| Product | Vendor | Free? |
|---------|--------|-------|
| **VirtualBox** | Oracle | Yes |
| **VMware Workstation** | VMware (Broadcom) | Paid |
| **VMware Fusion** | VMware (Broadcom) | Paid |
| **Parallels Desktop** | Parallels | Paid |
| **QEMU** | Open-source | Yes |

### When to use Type 2

- **Development** — Run a Linux VM on your macOS or Windows laptop
- **Testing** — Try a new OS without installing it
- **Learning** — Practice system administration safely

### Limitations

Since the hypervisor runs on top of an existing OS, there is an extra layer of overhead. The host OS consumes resources (RAM, CPU) that could otherwise go to VMs. Also, if the host OS crashes, **all VMs go down**.

## Type 1: The "Bare-Metal" Hypervisor

A Type 1 hypervisor runs **directly on the hardware**, replacing the traditional operating system entirely. It is purpose-built for one thing: running virtual machines.

```
┌─────────┐ ┌─────────┐ ┌─────────┐
│  VM-A   │ │  VM-B   │ │  VM-C   │
└────┬────┘ └────┬────┘ └────┬────┘
┌────┴───────────┴───────────┴────┐
│      Type 1 Hypervisor          │  ← IS the OS
├─────────────────────────────────┤
│      Physical Hardware          │
└─────────────────────────────────┘
```

There is no "host operating system" in the traditional sense. The hypervisor **is** the operating system — a minimal, specialized one designed exclusively for virtualization.

### Examples

| Product | Vendor | Open-source? |
|---------|--------|-------------|
| **VMware ESXi** | VMware (Broadcom) | Free tier available |
| **KVM** | Linux community | Yes (part of Linux kernel) |
| **Microsoft Hyper-V** | Microsoft | Included in Windows Server |
| **Xen** | Linux Foundation | Yes |
| **Proxmox VE** | Proxmox | Yes (based on KVM) |

### When to use Type 1

- **Production servers** — Maximum performance and reliability
- **Data centers** — Hundreds of VMs on powerful servers
- **Cloud infrastructure** — AWS, Azure, Google Cloud all use Type 1 hypervisors

### Advantages

- **Better performance** — No host OS overhead
- **Better security** — Smaller attack surface
- **Better stability** — If one VM crashes, the hypervisor and other VMs are unaffected

## The Special Case of KVM

KVM (Kernel-based Virtual Machine) blurs the line between Type 1 and Type 2. It is a **Linux kernel module** that turns the Linux kernel itself into a Type 1 hypervisor.

```
┌─────────┐ ┌─────────┐
│  VM-A   │ │  VM-B   │
└────┬────┘ └────┬────┘
┌────┴───────────┴────┐
│   Linux + KVM       │  ← Linux IS the hypervisor
├─────────────────────┤
│   Physical Hardware │
└─────────────────────┘
```

Is it Type 1 or Type 2? Technically, it's **Type 1** because the hypervisor (KVM) runs in kernel space with direct hardware access. But it also looks like Type 2 because you can still use the Linux host for other tasks.

KVM is the hypervisor that powers **most of the modern cloud**: Google Cloud, Oracle Cloud, and large parts of AWS and Azure use KVM. OpenStack, the open-source cloud platform, also uses KVM by default.

## Summary Table

| Feature | Type 1 (Bare-Metal) | Type 2 (Hosted) |
|---------|---------------------|-----------------|
| Runs on | Directly on hardware | On top of an OS |
| Performance | Excellent | Good |
| Use case | Servers, data centers | Desktops, development |
| Examples | ESXi, KVM, Hyper-V | VirtualBox, Workstation |
| Management | Remote (web/SSH) | Local GUI |
