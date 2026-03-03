---
title: "TCP/IP Quiz"
type: quiz
estimated_duration: "10m"
---

# TCP/IP Quiz

## [multiple-choice] What is the range of a private IPv4 address starting with 192.168?

- [ ] 192.168.0.0 – 192.168.0.255
- [x] 192.168.0.0 – 192.168.255.255
- [ ] 192.0.0.0 – 192.255.255.255
- [ ] 192.168.1.0 – 192.168.1.255

> **Explanation:** The private range 192.168.0.0/16 covers all addresses from 192.168.0.0 to 192.168.255.255, providing 65,534 usable host addresses.

## [short-answer] What does the IP address 127.0.0.1 represent?

    localhost

> **Explanation:** 127.0.0.1 is the loopback address, also called localhost. It refers to the machine itself — useful for testing network services locally.

## [multiple-choice] What does NAT (Network Address Translation) do?

- [ ] Encrypts network traffic
- [ ] Assigns MAC addresses to devices
- [x] Translates private IP addresses to a public IP address for internet access
- [ ] Converts IPv4 addresses to IPv6

> **Explanation:** NAT allows multiple devices with private IPs to share a single public IP address. The router tracks which internal device made which request to route responses back correctly.

## [multiple-choice] What does CIDR notation /24 mean?

- [ ] 24 devices can connect to the network
- [ ] The network has 24 routers
- [x] The first 24 bits identify the network, leaving 8 bits for hosts (254 usable addresses)
- [ ] The network speed is 24 Mbps

> **Explanation:** /24 means the subnet mask is 255.255.255.0. The first 24 bits define the network, and the remaining 8 bits define hosts, giving 254 usable addresses (256 minus network and broadcast).

## [multiple-choice] What is the TCP three-way handshake?

- [ ] A method to encrypt data
- [x] SYN → SYN-ACK → ACK, the process that establishes a TCP connection
- [ ] A way to split data into three packets
- [ ] An authentication protocol

> **Explanation:** Before exchanging data, TCP establishes a connection: the client sends SYN, the server responds with SYN-ACK, and the client confirms with ACK. Only then does data flow.

## [multiple-choice] Which protocol should you use for a video streaming application?

- [ ] TCP, because every frame must be delivered
- [x] UDP, because speed matters more than occasional lost frames
- [ ] Neither — streaming uses a special protocol
- [ ] TCP for video, UDP for audio

> **Explanation:** UDP is preferred for streaming because a few lost frames are imperceptible to viewers, while TCP's retransmission delays would cause visible buffering and lag.

## [multiple-choice] What is a port number used for?

- [ ] Identifying the physical network cable
- [ ] Determining the IP address
- [x] Identifying which application on a server should receive the network traffic
- [ ] Measuring network speed

> **Explanation:** Ports distinguish between different services on the same machine. A server's web server listens on port 80/443, its SSH on port 22, and its database on port 5432 — all on the same IP address.

## [short-answer] What well-known port does HTTPS use?

    443

> **Explanation:** HTTPS (HTTP over TLS) uses port 443 by default. HTTP without encryption uses port 80.
