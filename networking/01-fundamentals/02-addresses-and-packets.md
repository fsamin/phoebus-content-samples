---
title: "Addresses and Packets"
type: lesson
estimated_duration: "20m"
---

# Addresses and Packets

## Every Device Needs an Address

For devices to communicate, they need a way to identify each other — just like houses need addresses for mail delivery. Networks use two types of addresses.

### MAC Address (Physical Address)

Every network interface (Ethernet port, Wi-Fi adapter) has a unique **MAC address** (Media Access Control) burned into it at the factory.

```
Example: 3C:22:FB:A1:09:5E
```

- 48 bits, written as 6 pairs of hexadecimal digits
- **Unique worldwide** — no two devices share the same MAC address
- Used for communication **within a local network** (LAN)
- Cannot be used across the internet

Think of the MAC address as a device's **birth certificate number** — permanent and unique, but not useful for routing mail across the country.

### IP Address (Logical Address)

An **IP address** is a logical address assigned to a device by the network administrator or automatically (via DHCP).

```
IPv4 example: 192.168.1.42
IPv6 example: 2001:0db8:85a3:0000:0000:8a2e:0370:7334
```

- Used for communication **across networks** (including the internet)
- Can change when you move to a different network
- Like a **postal address** — tells routers where to deliver data

We'll dive deeper into IP addresses in the next module.

### MAC vs IP — Why Both?

| Feature | MAC Address | IP Address |
|---------|-------------|------------|
| Assigned by | Manufacturer | Network configuration |
| Scope | Local network only | Global (internet) |
| Changes? | No (permanent) | Yes (can change) |
| Used by | Switches | Routers |
| Analogy | Birth certificate number | Postal address |

When you send data across the internet, the **IP address** gets it to the right network, and the **MAC address** gets it to the right device within that network.

## How Data Travels: Packets

When you send a file over a network, it is **not sent as one big chunk**. Instead, it is broken into small pieces called **packets**.

### Why Packets?

Imagine a highway with only one lane. If someone sends a huge file as a single block, it would block the entire highway until the transfer is complete. No one else could use the network.

By breaking data into small packets (typically 1,500 bytes each), **multiple conversations can share the same network** — packets from different senders are interleaved.

```
File: "Hello, World!" (simplified)

Split into packets:
┌────────────┐  ┌────────────┐  ┌────────────┐
│ Packet 1   │  │ Packet 2   │  │ Packet 3   │
│ "Hell"     │  │ "o, W"     │  │ "orld!"    │
│ From: A    │  │ From: A    │  │ From: A    │
│ To: B      │  │ To: B      │  │ To: B      │
│ Seq: 1/3   │  │ Seq: 2/3   │  │ Seq: 3/3   │
└────────────┘  └────────────┘  └────────────┘
```

Each packet contains:
- **Source address** — Who sent it
- **Destination address** — Where it's going
- **Sequence number** — Its position in the original data
- **Payload** — The actual data

### Packets Can Take Different Routes

Packets don't necessarily follow the same path. Routers make independent decisions for each packet based on current network conditions:

```
Packet 1: A → Router1 → Router3 → B
Packet 2: A → Router2 → Router4 → B
Packet 3: A → Router1 → Router4 → B
```

The destination reassembles them in the correct order using sequence numbers.

## The Network Layer Model

To manage complexity, networking is organized in **layers**. Each layer handles one aspect of communication and uses the layer below it.

The most practical model is the **TCP/IP model** with 4 layers:

```
┌─────────────────────┐
│  4. Application     │  HTTP, DNS, SSH, SMTP
├─────────────────────┤
│  3. Transport       │  TCP, UDP
├─────────────────────┤
│  2. Internet        │  IP (addressing, routing)
├─────────────────────┤
│  1. Network Access  │  Ethernet, Wi-Fi (physical delivery)
└─────────────────────┘
```

Think of it like sending a letter:

| Layer | Letter Analogy |
|-------|---------------|
| **Application** | The letter's content (your message) |
| **Transport** | Choosing registered mail vs regular mail (reliable vs fast) |
| **Internet** | The postal address on the envelope |
| **Network Access** | The postal truck and roads that carry it |

Each layer adds its own **header** (metadata) to the packet as it passes through. This process is called **encapsulation**:

```
Application data:    [HTTP request]
Transport adds:      [TCP header][HTTP request]
Internet adds:       [IP header][TCP header][HTTP request]
Network Access adds: [Eth header][IP header][TCP header][HTTP request][Eth trailer]
```

At the receiving end, each layer strips off its header and passes the remaining data up — this is **decapsulation**.

You don't need to memorize every detail of this model right now. The important takeaway is that **networking works in layers**, and each layer has a specific job.
