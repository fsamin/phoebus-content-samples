---
title: "Understanding Virtualization Quiz"
type: quiz
estimated_duration: "10m"
---

# Understanding Virtualization Quiz

## [multiple-choice] What is virtualization?

- [ ] A way to make a computer run faster
- [x] A technology that allows multiple operating systems to run on a single physical machine
- [ ] A cloud storage service
- [ ] A type of operating system

> **Explanation:** Virtualization allows you to create multiple Virtual Machines (VMs) on one physical server, each running its own independent operating system.

## [multiple-choice] What is the main problem that virtualization solves?

- [ ] Computers are too expensive
- [ ] Internet connections are too slow
- [x] Physical servers are underutilized, wasting resources
- [ ] Operating systems have too many bugs

> **Explanation:** Before virtualization, most servers used less than 15% of their capacity. Virtualization allows running multiple workloads on one machine, dramatically improving utilization.

## [multiple-choice] What is a hypervisor?

- [ ] A very powerful computer
- [ ] A type of operating system
- [x] The software layer that creates and manages virtual machines
- [ ] A network device

> **Explanation:** The hypervisor sits between the physical hardware and the virtual machines. It manages resource sharing and ensures VMs are isolated from each other.

## [multiple-choice] Who invented virtualization?

- [ ] VMware in 1999
- [ ] Google in 2005
- [ ] Microsoft in 2008
- [x] IBM in the 1960s

> **Explanation:** IBM created the first virtualization system (CP/CMS) in 1967 for mainframes. VMware later brought virtualization to x86 servers in 1999.

## [multiple-choice] What hardware feature enabled efficient x86 virtualization?

- [ ] More RAM
- [ ] Faster hard drives
- [x] Intel VT-x and AMD-V CPU extensions
- [ ] Gigabit Ethernet

> **Explanation:** Intel VT-x (2005) and AMD-V (2006) added a new CPU privilege level (Ring -1) that allows the hypervisor to run below the guest OS kernel, eliminating the need for slow software-based virtualization tricks.

## [multiple-choice] What is thin provisioning?

- [ ] Using less RAM for virtual machines
- [ ] Reducing the number of virtual CPUs
- [x] A virtual disk that only uses physical space as data is actually written
- [ ] A way to compress virtual machine files

> **Explanation:** With thin provisioning, a 100 GB virtual disk starts small and grows as data is written. This allows overcommitting storage — allocating more total disk space than physically available.

## [short-answer] What is the physical machine that hosts virtual machines called?

    host

> **Explanation:** The physical machine is called the **host**, and the operating systems running inside VMs are called **guests**.

## [multiple-choice] What is the typical CPU overhead of a modern virtual machine?

- [x] 2–5%
- [ ] 20–30%
- [ ] 50%
- [ ] No overhead at all

> **Explanation:** With hardware-assisted virtualization (Intel VT-x/AMD-V), CPU overhead is typically only 2–5%, making VMs practical for nearly any workload.
