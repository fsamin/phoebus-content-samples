---
title: "VM Lifecycle in vSphere"
type: lesson
estimated_duration: "20m"
---

# VM Lifecycle in vSphere

## Creating a Virtual Machine

Creating a VM in vSphere involves specifying its virtual hardware:

| Setting | Example Value | Description |
|---------|--------------|-------------|
| **Name** | `web-server-01` | Human-readable identifier |
| **Guest OS** | Ubuntu 22.04 | Tells ESXi which optimizations to apply |
| **CPUs** | 4 vCPUs | Number of virtual CPU cores |
| **Memory** | 8 GB | RAM allocated to the VM |
| **Disk** | 100 GB (thin) | Virtual hard drive |
| **Network** | `VM Network` | Which virtual switch to connect to |
| **Datastore** | `shared-ssd-01` | Where to store the VM files |

Once created, the VM appears in the vSphere Client inventory as a "powered off" machine ready to start.

## VM Files

Every VM in vSphere is a collection of files on the datastore:

```
/vmfs/volumes/datastore1/web-server-01/
├── web-server-01.vmx         ← Configuration file (text)
├── web-server-01.vmdk        ← Disk descriptor
├── web-server-01-flat.vmdk   ← Actual disk data (large)
├── web-server-01.nvram       ← BIOS/UEFI settings
├── web-server-01.vmsd        ← Snapshot metadata
└── web-server-01.log         ← VM log file
```

The `.vmx` file is a simple text file containing all the VM settings. The `-flat.vmdk` file is where the actual guest data lives — this is typically the largest file.

Because a VM is just files, you can:
- **Copy** it to create a clone
- **Back it up** like any other file
- **Move** it to different storage

## Installing the Operating System

After creating the VM, you need to install an OS. Three common methods:

### 1. ISO Image
Upload an ISO file (e.g., `ubuntu-22.04-server.iso`) to the datastore and mount it as a virtual CD-ROM. The VM boots from it like a physical server would boot from a DVD.

### 2. PXE Network Boot
Configure the VM to boot from the network. A PXE server provides the OS installer automatically. This is common in large-scale deployments.

### 3. Template Cloning (Recommended)
Clone an existing template that already has the OS installed and configured. This is the fastest method — a new VM is ready in **seconds** instead of the 20–30 minutes an installation takes.

## VMware Tools

After installing the OS, you should install **VMware Tools** inside the guest. VMware Tools is a set of drivers and utilities that improve VM performance and integration:

- **Paravirtualized drivers** — Faster disk and network I/O
- **Time synchronization** — Keep the guest clock in sync with the host
- **Graceful shutdown** — Allow vCenter to cleanly shut down the guest OS
- **Mouse integration** — Seamless cursor movement in the console
- **Heartbeat** — Let vCenter know the guest OS is alive and healthy

> Without VMware Tools, your VM will work, but with degraded performance and limited management capabilities.

## VM Operations

| Operation | Description |
|-----------|-------------|
| **Power On** | Start the VM |
| **Power Off** | Force power off (like pulling the plug) |
| **Shut Down** | Graceful OS shutdown via VMware Tools |
| **Suspend** | Freeze the VM state to disk (like laptop sleep) |
| **Reset** | Hard reboot |
| **Snapshot** | Save current state as a restore point |
| **Clone** | Create an identical copy |
| **Migrate** | Move to another host (vMotion) or datastore |
| **Export** | Export as OVF/OVA for portability |

## Resource Monitoring

vSphere provides real-time and historical performance data for every VM:

- **CPU usage** — How much of its allocated vCPUs the VM is using
- **Memory usage** — Active vs. allocated vs. consumed memory
- **Disk I/O** — Read/write throughput and latency
- **Network I/O** — Traffic in/out in Mbps

This data helps you right-size VMs: if a VM was given 8 GB of RAM but never uses more than 2 GB, you can safely reduce its allocation to free resources for other VMs.
