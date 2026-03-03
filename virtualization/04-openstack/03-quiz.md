---
title: "OpenStack Quiz"
type: quiz
estimated_duration: "10m"
---

# OpenStack Quiz

## [multiple-choice] What is OpenStack?

- [ ] A commercial hypervisor made by VMware
- [ ] A Linux distribution
- [x] An open-source platform for building and managing cloud infrastructure
- [ ] A container orchestration tool

> **Explanation:** OpenStack is an open-source software platform that turns physical servers, storage, and networking into a self-service cloud, similar to AWS but running in your own data center.

## [multiple-choice] Which OpenStack service manages virtual machines?

- [ ] Neutron
- [ ] Keystone
- [x] Nova
- [ ] Cinder

> **Explanation:** Nova is the compute service — it receives instance launch requests, schedules them on physical hosts, and communicates with the hypervisor (usually KVM) to create and manage VMs.

## [multiple-choice] What does Keystone do?

- [ ] Manages virtual networks
- [x] Handles authentication and authorization for all OpenStack services
- [ ] Stores OS images
- [ ] Creates block storage volumes

> **Explanation:** Keystone is the identity service. Every request to any OpenStack service must first authenticate through Keystone to get a token proving the user's identity and permissions.

## [multiple-choice] What is the default hypervisor used by OpenStack?

- [ ] VMware ESXi
- [ ] Xen
- [x] KVM
- [ ] VirtualBox

> **Explanation:** KVM (Kernel-based Virtual Machine) is the default and most widely used hypervisor in OpenStack deployments. OpenStack can also work with ESXi and Xen, but KVM is the standard.

## [short-answer] What is the name of OpenStack's web-based user interface?

    horizon

> **Explanation:** Horizon is OpenStack's dashboard — a web interface where users can launch instances, manage networks, upload images, and perform all cloud operations visually.

## [multiple-choice] What is the key difference between vSphere and OpenStack provisioning models?

- [ ] vSphere is faster than OpenStack
- [ ] OpenStack only works with Linux
- [x] vSphere is admin-managed while OpenStack provides self-service for end users
- [ ] vSphere is open-source and OpenStack is commercial

> **Explanation:** In vSphere, admins create VMs on behalf of users. In OpenStack, users provision their own infrastructure through a portal or API — no admin intervention needed. This self-service model is what makes OpenStack a cloud platform.

## [multiple-choice] What does Cinder provide?

- [ ] Virtual networks
- [ ] User authentication
- [ ] OS images
- [x] Persistent block storage volumes that can be attached to instances

> **Explanation:** Cinder provides virtual hard drives (volumes) that persist independently of instances. You can detach a volume from one instance and attach it to another, preserving the data.

## [multiple-choice] Which organizations originally created OpenStack?

- [ ] Google and Microsoft
- [ ] VMware and Oracle
- [x] NASA and Rackspace
- [ ] Red Hat and Canonical

> **Explanation:** OpenStack was born in 2010 from NASA's Nebula cloud project and Rackspace's cloud storage software. They open-sourced their work, and it grew into the largest open-source cloud project.
