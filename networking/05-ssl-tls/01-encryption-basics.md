---
title: "Encryption Basics"
type: lesson
estimated_duration: "15m"
---

# Encryption Basics

Before understanding TLS, you need to grasp two fundamental encryption concepts.

## The Problem

When you send data over a network, it passes through many devices: routers, switches, ISP equipment. Anyone along the path could **read** or **modify** your data. This is called a **man-in-the-middle** attack.

```
You ──── Router ──── ISP ──── Server
              │
         Attacker can see
         your data here
```

Without encryption, your passwords, credit card numbers, and private messages travel in **plain text** — readable by anyone who intercepts them.

## Symmetric Encryption

**Symmetric encryption** uses the **same key** to encrypt and decrypt.

```
"Hello" + Key → "x7#Qp"  (encrypt)
"x7#Qp" + Key → "Hello"  (decrypt)
```

It's fast and efficient, but has a problem: **how do you share the key with the other person securely?** If you send the key over the network, an attacker can intercept it.

Common algorithms: **AES** (AES-128, AES-256), ChaCha20.

## Asymmetric Encryption (Public Key Cryptography)

**Asymmetric encryption** uses a **pair of keys**: a public key and a private key.

- **Public key** — Can be shared with anyone. Used to **encrypt** data.
- **Private key** — Kept secret. Used to **decrypt** data.

```
Sender has: Bob's public key
Bob has: Bob's private key

"Hello" + Bob's public key → "x7#Qp"  (encrypt)
"x7#Qp" + Bob's private key → "Hello" (decrypt)
```

The magic: **data encrypted with the public key can only be decrypted with the private key**. Even the person who encrypted it cannot decrypt it without the private key.

This solves the key-sharing problem: Bob publishes his public key openly. Anyone can encrypt a message for Bob, but only Bob (with his private key) can read it.

Common algorithms: **RSA**, **ECDSA**, Ed25519.

## The Hybrid Approach

Asymmetric encryption is **slow** — too slow for encrypting large amounts of data. Symmetric encryption is **fast** but has the key-sharing problem.

TLS uses **both**:

1. Use **asymmetric encryption** to securely exchange a symmetric key
2. Use **symmetric encryption** for all subsequent data (fast)

```
1. Client + Server use asymmetric crypto to agree on a shared secret
2. Both derive a symmetric key from the shared secret
3. All data is encrypted with the fast symmetric key
```

This is exactly what happens during the TLS handshake.
