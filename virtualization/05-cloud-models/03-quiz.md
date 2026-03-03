---
title: "Cloud Models Quiz"
type: quiz
estimated_duration: "10m"
---

# Cloud Models Quiz

## [multiple-choice] What is a private cloud?

- [ ] A cloud that nobody knows about
- [x] Cloud infrastructure operated solely for one organization
- [ ] A free tier on AWS
- [ ] A VPN connection to the internet

> **Explanation:** A private cloud is cloud infrastructure dedicated to a single organization. The hardware may be owned or leased, but the resources are not shared with other organizations.

## [multiple-choice] What is the main advantage of public cloud?

- [ ] It is always cheaper than private cloud
- [ ] It is more secure than private cloud
- [x] No upfront cost, on-demand scalability, and no hardware maintenance
- [ ] It gives you full control over the hardware

> **Explanation:** Public cloud eliminates the need to buy hardware. You pay per use, can scale instantly, and the provider handles all maintenance. However, you trade control and data sovereignty for these benefits.

## [multiple-choice] What is hybrid cloud?

- [ ] A cloud that uses both Linux and Windows
- [ ] A cloud that is half public and half private
- [x] A combination of private and public cloud with workloads able to move between them
- [ ] A cloud operated by two different providers

> **Explanation:** Hybrid cloud connects private and public cloud environments, allowing organizations to place workloads where they best fit — sensitive data on-premises, scalable workloads in public cloud.

## [multiple-choice] Which cloud service model provides virtual machines, networks, and storage?

- [x] IaaS (Infrastructure as a Service)
- [ ] PaaS (Platform as a Service)
- [ ] SaaS (Software as a Service)
- [ ] FaaS (Function as a Service)

> **Explanation:** IaaS provides the fundamental infrastructure layer: virtual machines, networking, and storage. vSphere and OpenStack are IaaS solutions. You still manage the OS, applications, and data.

## [multiple-choice] What is the main strength of vSphere for private cloud?

- [ ] It is free and open-source
- [x] Mature, proven, with strong enterprise support and a rich ecosystem
- [ ] It provides self-service for developers
- [ ] It uses KVM as its hypervisor

> **Explanation:** vSphere has 25+ years of development, is battle-tested in production, and comes with enterprise support and a rich ecosystem of tools for backup, monitoring, and automation.

## [multiple-choice] What is the main strength of OpenStack for cloud infrastructure?

- [ ] It is simpler to deploy than vSphere
- [ ] It only works with VMware hardware
- [x] Open-source, API-driven, self-service, with no vendor lock-in
- [ ] It comes with free 24/7 support

> **Explanation:** OpenStack is open-source (no license costs), designed for self-service from the ground up, fully API-driven for automation, and uses open standards — avoiding vendor lock-in.

## [short-answer] What NIST characteristic of cloud computing means users can get resources without human intervention?

    on-demand self-service

> **Explanation:** On-demand self-service means users can provision resources (VMs, storage, networks) automatically through a portal or API, without needing to contact an administrator.

## [multiple-choice] Which of the following is an example of SaaS?

- [ ] AWS EC2
- [ ] OpenStack
- [ ] Heroku
- [x] Gmail

> **Explanation:** Gmail is Software as a Service — you use the email application directly without managing any infrastructure, platform, or software installation. EC2 is IaaS, Heroku is PaaS.

## [multiple-choice] In a hybrid cloud scenario, what does "cloud bursting" mean?

- [ ] A cloud server crashing due to overload
- [ ] Migrating all workloads permanently to public cloud
- [x] Running baseline workloads on private cloud and expanding to public cloud during peak demand
- [ ] Using two public cloud providers simultaneously

> **Explanation:** Cloud bursting is a hybrid cloud pattern where normal workloads run on private cloud, but during traffic spikes or peak demand, additional capacity is provisioned in the public cloud automatically.
