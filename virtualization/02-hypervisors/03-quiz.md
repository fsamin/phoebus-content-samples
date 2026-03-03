---
title: "Hypervisors Quiz"
type: quiz
estimated_duration: "10m"
---

# Hypervisors Quiz

## [multiple-choice] What is a Type 1 (bare-metal) hypervisor?

- [ ] A hypervisor that runs as an application on Windows or macOS
- [x] A hypervisor that runs directly on the physical hardware
- [ ] A hypervisor that only works with Intel CPUs
- [ ] A hypervisor that can only run one VM at a time

> **Explanation:** A Type 1 hypervisor runs directly on the hardware with no host operating system in between. It is purpose-built for running virtual machines. Examples: VMware ESXi, KVM, Hyper-V.

## [multiple-choice] Which of the following is a Type 2 hypervisor?

- [ ] VMware ESXi
- [ ] KVM
- [x] VirtualBox
- [ ] Microsoft Hyper-V

> **Explanation:** VirtualBox is a Type 2 (hosted) hypervisor — it runs as an application on top of a host operating system (Windows, macOS, or Linux).

## [multiple-choice] What is special about KVM?

- [ ] It only runs on Windows
- [ ] It is the most expensive hypervisor
- [x] It is a Linux kernel module that turns Linux into a Type 1 hypervisor
- [ ] It does not support live migration

> **Explanation:** KVM (Kernel-based Virtual Machine) is built directly into the Linux kernel. It turns the Linux kernel itself into a hypervisor, giving it direct hardware access like a Type 1 hypervisor.

## [multiple-choice] What is a VM snapshot?

- [ ] A photo of the server hardware
- [ ] A compressed backup stored off-site
- [x] A capture of a VM's complete state at a specific moment that can be reverted to
- [ ] A log of all actions performed on the VM

> **Explanation:** A snapshot saves the VM's disk, memory, and configuration state. You can revert to it if something goes wrong, like a save point in a video game.

## [multiple-choice] What is live migration?

- [ ] Moving a VM's backup to another data center
- [x] Moving a running VM from one physical host to another without downtime
- [ ] Upgrading a VM's operating system
- [ ] Cloning a VM to create a copy

> **Explanation:** Live migration (vMotion in VMware) transfers a running VM to another host by copying its memory in real-time. The VM experiences only milliseconds of pause.

## [short-answer] What hypervisor feature automatically restarts VMs on another host when hardware fails?

    high availability

> **Explanation:** High Availability (HA) monitors host health and automatically restarts VMs on surviving hosts when a failure is detected. Unlike live migration, it involves a brief reboot.

## [multiple-choice] Why are templates useful in virtualization?

- [ ] They make VMs run faster
- [ ] They reduce storage costs
- [x] They allow creating pre-configured VMs in seconds instead of installing from scratch
- [ ] They encrypt virtual machine disks

> **Explanation:** Templates are pre-built VM images. You clone them to create new VMs almost instantly, ensuring consistency across deployments.
