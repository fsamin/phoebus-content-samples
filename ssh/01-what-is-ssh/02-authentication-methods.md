---
title: "Authentication Methods"
type: lesson
estimated_duration: "10m"
---

# Authentication Methods

SSH supports several ways to prove your identity. Understanding them is key to making the right security choices.

## Password Authentication

The simplest method: the server asks for your password, you type it.

```bash
ssh user@server.example.com
user@server.example.com's password: ********
```

### Why Passwords Are Weak

- **Brute-forceable** — Attackers can try thousands of passwords per minute
- **Phishable** — Users can be tricked into entering passwords on fake servers
- **Reusable** — People use the same password on multiple systems
- **Inconvenient** — Typing passwords every time is tedious

Password authentication should be **disabled in production environments**. We'll see how later.

## Public Key Authentication (Recommended)

This is the method used by professionals. Instead of a password, you prove your identity with a **cryptographic key pair**:

- **Private key** — Stays on your machine, never shared. Protected by a passphrase.
- **Public key** — Placed on every server you want to access. Safe to share openly.

```
Your machine                          Server
┌────────────────┐                ┌────────────────┐
│ Private key 🔑 │                │ Public key 🔓  │
│ (secret!)      │                │ (in             │
│                │                │  authorized_keys)│
└───────┬────────┘                └───────┬────────┘
        │                                │
        │──── "Prove you have the ──────►│
        │      matching private key"     │
        │                                │
        │◄─── Challenge ────────────────│
        │                                │
        │──── Signed response ─────────►│
        │     (using private key)        │
        │                                │
        │◄─── "Verified! Welcome" ──────│
```

The private key **never leaves your machine**. The server sends a challenge that only the matching private key can solve. If the response is correct, you're in.

### Why Keys Are Better

| Aspect | Password | SSH Key |
|--------|----------|---------|
| Brute-force resistance | Weak | Virtually impossible |
| Convenience | Type every time | Automatic (with agent) |
| Phishing risk | High | None (key never sent) |
| Automation | Difficult | Easy (scripts, CI/CD) |
| Can be shared? | Shouldn't be | Public key: yes |

## Certificate-Based Authentication

An advanced method used in large organizations: instead of placing public keys on every server, a **Certificate Authority (CA)** signs the keys. The servers trust the CA, and any key signed by it is accepted.

This scales better than managing `authorized_keys` files on hundreds of servers, but is more complex to set up. The Bastion (covered in Module 5) provides a similar centralized approach.

## Keyboard-Interactive (MFA)

Some setups require **multi-factor authentication**: a password plus a one-time code (TOTP, YubiKey). SSH supports this through the keyboard-interactive method, where the server can ask multiple questions.

```
ssh user@secure-server.example.com
Password: ********
Verification code: 482937
```

This is common in high-security environments and is supported by The Bastion.
