---
title: "IP Addressing"
type: lesson
estimated_duration: "20m"
---

# IP Addressing

## IPv4 Addresses

An **IPv4 address** is a 32-bit number, written as four numbers separated by dots. Each number ranges from 0 to 255.

```
192.168.1.42
```

That gives us about **4.3 billion** possible addresses (2³²). This sounds like a lot, but with billions of connected devices, we actually ran out of IPv4 addresses in 2011.

## Public vs Private Addresses

Not all IP addresses are visible on the internet. Some ranges are reserved for **private networks** (LANs):

| Range | Block | Typical Use |
|-------|-------|-------------|
| `10.0.0.0` – `10.255.255.255` | 10.0.0.0/8 | Large organizations |
| `172.16.0.0` – `172.31.255.255` | 172.16.0.0/12 | Medium networks |
| `192.168.0.0` – `192.168.255.255` | 192.168.0.0/16 | Home and small office |

Private addresses **cannot be used on the internet**. They are used inside your network and translated to a public address at the router via **NAT** (Network Address Translation).

```
Private network                    Internet
┌────────────────┐    NAT     
│ 192.168.1.10   │──┐             
│ 192.168.1.11   │──┤→ Router → 203.0.113.5 → Internet
│ 192.168.1.12   │──┘  (public IP)
└────────────────┘
```

All your devices share a single public IP address. The router keeps track of which internal device made which request.

## Subnet Masks

A **subnet mask** defines which part of an IP address identifies the **network** and which part identifies the **device** (host).

```
IP address:   192.168.1.42
Subnet mask:  255.255.255.0

Network part: 192.168.1.___  (same for all devices on this network)
Host part:    ___.___.___.42 (unique per device)
```

### CIDR Notation

Instead of writing `255.255.255.0`, we use **CIDR notation**: a slash followed by the number of network bits.

| Subnet Mask | CIDR | # of Hosts |
|-------------|------|------------|
| 255.255.255.0 | /24 | 254 |
| 255.255.0.0 | /16 | 65,534 |
| 255.0.0.0 | /8 | 16,777,214 |
| 255.255.255.128 | /25 | 126 |

Example: `192.168.1.0/24` means "all addresses from 192.168.1.0 to 192.168.1.255 belong to this network."

As a DevOps engineer, you'll see CIDR notation everywhere — in cloud VPC configurations, firewall rules, and Docker networks.

## Special Addresses

| Address | Purpose |
|---------|---------|
| `127.0.0.1` | **Localhost** — the machine talking to itself |
| `0.0.0.0` | "All interfaces" (listen on everything) |
| `255.255.255.255` | Broadcast to all devices on the network |
| `169.254.x.x` | Auto-assigned when DHCP fails (link-local) |

## IPv6 — The Future

IPv6 uses **128-bit addresses**, providing 340 undecillion addresses (3.4 × 10³⁸) — enough for every grain of sand on Earth.

```
IPv6 example: 2001:0db8:85a3:0000:0000:8a2e:0370:7334
Shortened:    2001:db8:85a3::8a2e:370:7334
```

IPv6 adoption is growing, but IPv4 is still dominant. Most systems run **dual-stack** — supporting both IPv4 and IPv6 simultaneously.

## DHCP: Automatic Address Assignment

Manually configuring IP addresses on every device would be impractical. **DHCP** (Dynamic Host Configuration Protocol) automates this:

1. Device connects to the network and broadcasts: "I need an IP address!"
2. DHCP server responds: "Here's `192.168.1.42`, subnet mask `255.255.255.0`, gateway `192.168.1.1`, DNS server `8.8.8.8`"
3. Device uses these settings automatically

DHCP is used everywhere: home routers, office networks, cloud environments. Servers, however, typically use **static (fixed) IP addresses** because their address should never change.
