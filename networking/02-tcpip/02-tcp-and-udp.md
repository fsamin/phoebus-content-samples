---
title: "TCP and UDP"
type: lesson
estimated_duration: "20m"
---

# TCP and UDP

IP handles **addressing and routing** — getting packets from A to B. But IP alone doesn't guarantee that packets arrive, arrive in order, or aren't duplicated. That's the job of the **transport layer**.

Two protocols handle this: **TCP** and **UDP**.

## Ports: Identifying Applications

A single server can run many services: a web server, a database, an SSH daemon. How does the network know which application should receive each packet?

**Ports.** Each network service listens on a specific port number (0–65535).

| Port | Service | Protocol |
|------|---------|----------|
| 22 | SSH | TCP |
| 53 | DNS | UDP (and TCP) |
| 80 | HTTP | TCP |
| 443 | HTTPS | TCP |
| 5432 | PostgreSQL | TCP |
| 3306 | MySQL | TCP |

A connection is identified by 4 elements: **source IP + source port + destination IP + destination port**.

```
Your laptop (192.168.1.10:54321) → Web server (93.184.216.34:443)
```

Ports below 1024 are called **well-known ports** and typically require administrator privileges to use.

## TCP: Reliable Delivery

**TCP** (Transmission Control Protocol) guarantees that data arrives **completely, in order, and without errors**.

### The Three-Way Handshake

Before any data is sent, TCP establishes a connection with a **three-way handshake**:

```
Client                    Server
  │                         │
  │──── SYN ──────────────►│  "I want to connect"
  │                         │
  │◄──── SYN-ACK ──────────│  "OK, I'm ready too"
  │                         │
  │──── ACK ──────────────►│  "Great, let's go"
  │                         │
  │◄════ Data exchange ════►│
```

This ensures both sides are ready before sending data.

### How TCP Ensures Reliability

1. **Sequence numbers** — Every byte is numbered so the receiver can reassemble data in order
2. **Acknowledgments** — The receiver confirms each chunk received ("I got bytes 1–1000")
3. **Retransmission** — If no acknowledgment arrives within a timeout, the sender resends
4. **Checksums** — Detects corrupted data
5. **Flow control** — The receiver tells the sender to slow down if it's overwhelmed

### Connection Teardown

When done, TCP closes the connection cleanly:

```
Client                    Server
  │──── FIN ──────────────►│  "I'm done sending"
  │◄──── ACK ──────────────│  "OK"
  │◄──── FIN ──────────────│  "I'm done too"
  │──── ACK ──────────────►│  "OK, goodbye"
```

### When to Use TCP

TCP is used when **data must arrive perfectly**: web pages, file downloads, emails, database connections, SSH sessions. The overhead of reliability is worth it.

## UDP: Fast but Unreliable

**UDP** (User Datagram Protocol) is the opposite of TCP. It sends data without establishing a connection and without guarantees.

```
Client                    Server
  │                         │
  │──── Data ─────────────►│  "Here's some data"
  │──── Data ─────────────►│  "And some more"
  │──── Data ─────────────►│  "And more"
  │                         │
  (no handshake, no confirmation, no retransmission)
```

### Characteristics

- **No connection setup** — Just send
- **No guaranteed delivery** — Packets can be lost
- **No ordering** — Packets can arrive out of order
- **No congestion control** — Sender blasts as fast as it wants
- **Low overhead** — Minimal header (8 bytes vs TCP's 20+ bytes)

### When to Use UDP

UDP is used when **speed matters more than perfection**:

| Use Case | Why UDP? |
|----------|----------|
| **DNS queries** | Small, fast lookups; can just retry if lost |
| **Video streaming** | A few dropped frames are invisible |
| **Online gaming** | Old position data is useless; need latest update fast |
| **VoIP (voice calls)** | A brief audio glitch is better than delay |

## TCP vs UDP Summary

| Feature | TCP | UDP |
|---------|-----|-----|
| Connection | Yes (handshake) | No |
| Reliability | Guaranteed delivery | Best effort |
| Ordering | In order | No guarantee |
| Speed | Slower (overhead) | Faster |
| Use cases | Web, email, files, SSH | DNS, streaming, gaming |
| Header size | 20+ bytes | 8 bytes |

> **Rule of thumb:** If losing a packet is unacceptable → TCP. If losing a packet is tolerable and speed matters → UDP.
