---
title: "What is a Network?"
type: lesson
estimated_duration: "15m"
---

# What is a Network?

## The Simplest Definition

A **computer network** is two or more devices connected together so they can exchange data.

That's it. Two laptops connected with a cable? That's a network. Your phone talking to your Wi-Fi router? That's a network. Billions of devices connected worldwide? That's the internet — the biggest network of all.

## Why Do We Need Networks?

Before networks, if you wanted to share a file with a colleague, you would copy it onto a floppy disk and physically walk it over. This was actually called **"sneakernet"**.

Networks exist to:

1. **Share data** — Send files, messages, emails
2. **Share resources** — Printers, storage, internet access
3. **Communicate** — Video calls, chat, collaboration
4. **Access services** — Web applications, databases, APIs

## Types of Networks

Networks are classified by their **size**:

| Type | Name | Range | Example |
|------|------|-------|---------|
| **PAN** | Personal Area Network | ~1 meter | Bluetooth between phone and headphones |
| **LAN** | Local Area Network | Building/campus | Office network, home Wi-Fi |
| **WAN** | Wide Area Network | Cities/countries | Corporate network between offices |
| **Internet** | The Internet | Global | The worldwide network of networks |

As a DevOps engineer, you will mostly work with **LANs** (data center networks) and the **internet**.

## How Devices Connect

### Wired Connections

The most common cable is **Ethernet** (technically, twisted-pair copper cable with RJ45 connectors). You've seen these — they look like a thick phone cable.

| Standard | Speed | Common Name |
|----------|-------|-------------|
| Cat 5e | 1 Gbps | Gigabit Ethernet |
| Cat 6 | 10 Gbps | 10-Gigabit Ethernet |
| Cat 6a | 10 Gbps | 10G (longer distance) |
| Fiber optic | 10–400 Gbps | Fiber |

Data centers use Ethernet cables and fiber optics. Fiber is used for longer distances and higher speeds.

### Wireless Connections

**Wi-Fi** (IEEE 802.11) transmits data over radio waves. Convenient for mobility, but slower and less reliable than wired connections.

In data centers and servers, **wired Ethernet is always preferred** for its reliability and speed.

## Network Devices

| Device | Role |
|--------|------|
| **Switch** | Connects devices within a LAN. Forwards data to the correct device based on its address |
| **Router** | Connects different networks together. Decides the best path for data to travel |
| **Access Point** | Provides Wi-Fi connectivity to wireless devices |
| **Firewall** | Filters traffic based on security rules. Blocks unauthorized access |

### A Simple Office Network

```
Internet
    │
┌───┴───┐
│Router │ ← connects office to internet
└───┬───┘
    │
┌───┴───┐
│Firewall│ ← blocks unwanted traffic
└───┬───┘
    │
┌───┴────┐
│ Switch  │ ← connects all devices together
└┬──┬──┬─┘
 │  │  │
 PC PC Printer
```

## Clients and Servers

In most networks, devices play one of two roles:

- **Client** — Requests a service (your browser asking for a web page)
- **Server** — Provides a service (the web server sending the page back)

This is the **client-server model**, and it is the foundation of nearly all internet services.

```
Client (browser)  ──── request ────►  Server (web app)
                  ◄── response ────
```

A single physical machine can be both a client and a server. Your laptop is a client when browsing the web, but it could also run a server that other devices connect to.
