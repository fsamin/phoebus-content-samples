---
title: "A Brief History"
type: lesson
estimated_duration: "15m"
---

# A Brief History of Virtualization

## The Mainframe Era (1960s–1970s)

Virtualization is not new. It was invented in the **1960s** by IBM.

Back then, computers were enormous machines called **mainframes** that filled entire rooms and cost millions of dollars. Only large corporations and universities could afford them.

The problem? These machines could only run **one program at a time**. If a scientist needed to run a calculation for 2 hours, everyone else had to wait. This was incredibly wasteful.

In 1967, IBM created **CP/CMS** (Control Program / Cambridge Monitor System) for the IBM System/360 Model 67. This system could divide the mainframe into **multiple virtual machines**, each running its own operating system independently. For the first time, multiple users could share the same physical hardware as if they each had their own computer.

## The Dark Ages (1980s–1990s)

When personal computers and x86 servers became affordable in the 1980s, virtualization was largely **forgotten**. Why share a machine when servers were cheap?

Companies adopted a **one application per server** model. It was simple, but it led to a massive problem: **server sprawl**. Data centers filled up with thousands of servers, most of them using less than 15% of their capacity.

## The VMware Revolution (1998–2000s)

In 1998, a company called **VMware** was founded in Palo Alto, California. Their goal: bring virtualization to the common x86 servers that everyone was using.

In 1999, VMware released **VMware Workstation**, allowing users to run virtual machines on their desktop computers. In 2001, they released **ESX Server** (later renamed ESXi), the first commercial hypervisor for x86 servers.

This was a game-changer. Suddenly, companies could consolidate 10, 20, or even 50 servers onto a single physical machine.

## The Open-Source Response (2003–2010s)

VMware's success inspired open-source alternatives:

| Year | Project | Contribution |
|------|---------|-------------|
| 2003 | **Xen** | Open-source hypervisor, used by early Amazon EC2 |
| 2006 | **KVM** | Built directly into the Linux kernel |
| 2007 | **VirtualBox** | Free desktop virtualization (now Oracle) |
| 2010 | **OpenStack** | Open-source cloud platform using KVM/Xen |

## Hardware Assistance (2005+)

A major breakthrough came when CPU manufacturers added **hardware virtualization support**:

- **Intel VT-x** (2005) — Intel's virtualization technology
- **AMD-V** (2006) — AMD's equivalent

Before these technologies, hypervisors had to use complex software tricks to intercept and translate CPU instructions. With hardware support, virtual machines could run at **near-native speed**, making virtualization practical for any workload.

## Timeline Summary

```
1967 ─── IBM CP/CMS (mainframes)
  │
1980s ── x86 PCs arrive, virtualization forgotten
  │
1999 ─── VMware Workstation released
2001 ─── VMware ESX (server virtualization)
2003 ─── Xen open-source hypervisor
2005 ─── Intel VT-x hardware support
2006 ─── KVM integrated into Linux
2010 ─── OpenStack launched
2013 ─── Docker (containers, a new form of virtualization)
  │
Today ── Cloud computing runs on virtualization
```

Understanding this history helps explain why the virtualization landscape looks the way it does today: VMware dominates the commercial market, KVM powers most open-source and cloud solutions, and newer technologies like containers build on top of these foundations.
