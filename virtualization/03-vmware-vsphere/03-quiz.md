---
title: "VMware vSphere Quiz"
type: quiz
estimated_duration: "10m"
---

# VMware vSphere Quiz

## [multiple-choice] What are the two main components of VMware vSphere?

- [ ] VMware Workstation and VMware Fusion
- [x] ESXi (hypervisor) and vCenter Server (management)
- [ ] VirtualBox and vSphere Client
- [ ] vSAN and NSX

> **Explanation:** vSphere is a suite built around ESXi (the Type 1 hypervisor installed on each server) and vCenter Server (the centralized management platform that controls all hosts).

## [multiple-choice] What is ESXi?

- [ ] A desktop virtualization application
- [ ] A cloud storage service
- [x] A Type 1 bare-metal hypervisor installed directly on server hardware
- [ ] A web browser for managing VMs

> **Explanation:** ESXi is VMware's bare-metal hypervisor. It is a minimal operating system (~150 MB) whose sole purpose is to run virtual machines with maximum performance.

## [multiple-choice] What is vMotion?

- [ ] A way to install VMware Tools
- [ ] A network monitoring tool
- [x] VMware's technology for live-migrating a running VM between hosts without downtime
- [ ] A backup solution

> **Explanation:** vMotion moves a running VM from one ESXi host to another by copying its memory in real-time. The VM experiences only milliseconds of pause, invisible to users.

## [multiple-choice] What does DRS (Distributed Resource Scheduler) do?

- [ ] Distributes network traffic across switches
- [ ] Manages DNS records for VMs
- [x] Automatically balances VM workloads across hosts in a cluster
- [ ] Schedules VM backups

> **Explanation:** DRS monitors CPU and memory usage across all hosts in a cluster. When imbalances are detected, it uses vMotion to migrate VMs from overloaded hosts to less busy ones.

## [multiple-choice] Why do vSphere clusters need shared storage?

- [ ] To save money on hard drives
- [ ] Because ESXi cannot use local disks
- [x] So that any host in the cluster can access any VM's disk files, enabling vMotion and HA
- [ ] To encrypt VM data

> **Explanation:** Features like vMotion and HA require that all hosts can access the same VM files. If VM disks were on a host's local storage, they could only run on that specific host.

## [short-answer] What software should be installed inside every vSphere VM for optimal performance?

    vmware tools

> **Explanation:** VMware Tools provides paravirtualized drivers for faster I/O, time synchronization, graceful shutdown support, and heartbeat monitoring.

## [multiple-choice] What is a vSphere cluster?

- [ ] A single ESXi host with many VMs
- [x] A group of ESXi hosts managed together by vCenter, enabling DRS, HA, and vMotion
- [ ] A type of shared storage
- [ ] A network of virtual switches

> **Explanation:** A cluster groups multiple ESXi hosts together. This enables enterprise features: DRS for automatic load balancing, HA for failover, and vMotion for live migration between any hosts in the cluster.
