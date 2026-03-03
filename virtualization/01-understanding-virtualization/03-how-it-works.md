---
title: "How Virtualization Works"
type: lesson
estimated_duration: "25m"
---

# How Virtualization Works

## The Core Challenge

An operating system expects to have **full control** over the hardware. It directly manages the CPU, the memory, and the disk. When you run two operating systems on the same machine, they both want exclusive access to the same resources.

The role of the **hypervisor** is to solve this conflict by sitting between the hardware and the virtual machines, managing resource sharing transparently.

## CPU Virtualization

### The Problem

CPUs have different privilege levels called **rings**:

```
┌─────────────────────────┐
│    Ring 3 — User apps    │  (least privileged)
├─────────────────────────┤
│    Ring 2 — Drivers      │
├─────────────────────────┤
│    Ring 1 — Drivers      │
├─────────────────────────┤
│    Ring 0 — OS Kernel    │  (most privileged)
└─────────────────────────┘
```

The OS kernel runs at **Ring 0** because it needs full hardware access. But if a VM's kernel also runs at Ring 0, it could interfere with the host or other VMs.

### The Solution

Modern CPUs (Intel VT-x, AMD-V) added a new level below Ring 0, sometimes called **Ring -1**:

```
┌─────────────────────────┐
│    Ring 3 — Guest apps   │
├─────────────────────────┤
│    Ring 0 — Guest kernel │
├─────────────────────────┤
│   Ring -1 — Hypervisor   │  ← controls everything
└─────────────────────────┘
```

The guest OS thinks it runs at Ring 0, but the hypervisor runs at an even more privileged level. When the guest tries to execute a sensitive operation, the CPU automatically **traps** it and hands control to the hypervisor, which decides what to do.

## Memory Virtualization

### The Problem

Each VM needs its own private memory space. If VM-A writes to memory address `0x1000`, it must not overwrite VM-B's data at the same address.

### The Solution

The hypervisor manages a **second layer of address translation**:

```
Guest Virtual Address  →  Guest Physical Address  →  Host Physical Address
  (what the app sees)     (what the guest OS sees)   (actual RAM location)
```

Modern CPUs provide hardware support for this:
- Intel calls it **EPT** (Extended Page Tables)
- AMD calls it **RVI** (Rapid Virtualization Indexing)

This means the translation happens in hardware, with almost no performance penalty.

## Storage Virtualization

A VM's hard drive is actually a **file on the host's disk**. Common formats:

| Format | Used By | Description |
|--------|---------|-------------|
| **VMDK** | VMware | Virtual Machine Disk |
| **QCOW2** | KVM/QEMU | QEMU Copy-On-Write, supports snapshots |
| **VDI** | VirtualBox | Virtual Disk Image |
| **VHD/VHDX** | Hyper-V | Microsoft's format |

When the guest OS reads or writes to its "disk", the hypervisor translates these operations into reads and writes on the host file. The guest has no idea it's not using a real disk.

### Thin Provisioning

You can create a 100 GB virtual disk that only uses **actual space as data is written**. If the VM has only written 20 GB of data, the file on the host is only ~20 GB. This is called **thin provisioning** and allows you to allocate more total storage than physically available (overcommit).

## Network Virtualization

Each VM needs its own network interface, IP address, and the ability to communicate with other VMs and the outside world.

The hypervisor creates **virtual switches** that connect VMs together:

```
┌─────────┐   ┌─────────┐   ┌─────────┐
│  VM-A   │   │  VM-B   │   │  VM-C   │
│ 10.0.0.1│   │ 10.0.0.2│   │ 10.0.0.3│
└────┬────┘   └────┬────┘   └────┬────┘
     │             │             │
┌────┴─────────────┴─────────────┴────┐
│         Virtual Switch               │
└─────────────────┬───────────────────┘
                  │
          ┌───────┴───────┐
          │ Physical NIC  │ → External Network
          └───────────────┘
```

VMs on the same virtual switch can communicate at memory speed (no physical network traffic). Traffic to the outside world goes through the physical network interface.

## Performance Impact

A common question: **"How much slower is a VM compared to bare metal?"**

With modern hardware-assisted virtualization:

| Resource | Overhead |
|----------|----------|
| CPU | 2–5% |
| Memory | 1–3% |
| Disk I/O | 5–15% |
| Network I/O | 3–10% |

For most workloads, virtualization overhead is **negligible**. The efficiency gains (running multiple VMs per server) far outweigh the small performance cost.
