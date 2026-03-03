---
title: "Key Types and Algorithms"
type: lesson
estimated_duration: "10m"
---

# Key Types and Algorithms

Before generating your first key pair, you need to understand the different algorithms available and which one to choose.

## Available Algorithms

| Algorithm | Key Size | Security | Speed | Recommendation |
|-----------|----------|----------|-------|---------------|
| **Ed25519** | 256 bits | Excellent | Fastest | ✅ **Best choice** |
| **ECDSA** | 256/384/521 bits | Very good | Fast | ✅ Good alternative |
| **RSA** | 2048–4096 bits | Good (with 4096) | Slower | ⚠️ Use 4096 bits minimum |
| **DSA** | 1024 bits | Weak | — | ❌ Deprecated, never use |

### Ed25519 (Recommended)

Ed25519 is the modern standard:
- **Smallest keys** — 256 bits (the public key fits on one line)
- **Fastest** — Key generation and authentication are nearly instant
- **Most secure** — Based on elliptic curve cryptography (Curve25519)
- **No configuration needed** — Only one key size, no bad choices possible

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGhKxP3Y... user@hostname
```

### RSA (Legacy Compatible)

RSA is the oldest algorithm still in use. Choose it only if you need to connect to **ancient systems** that don't support Ed25519.

Always use **4096 bits** — RSA 2048 is considered the minimum, and anything less is insecure.

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQ... (much longer)
```

## Key Files

When you generate a key pair, two files are created:

| File | Content | Permissions |
|------|---------|-------------|
| `~/.ssh/id_ed25519` | **Private key** — Never share! | `600` (owner read/write only) |
| `~/.ssh/id_ed25519.pub` | **Public key** — Safe to share | `644` (readable by all) |

The private key file looks like this:
```
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUA...
...several lines of base64...
-----END OPENSSH PRIVATE KEY-----
```

The public key is a single line:
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGhKxP3Y... user@hostname
```

## Passphrases

When generating a key, you're asked for a **passphrase**. This encrypts the private key file on disk.

- **Without passphrase** — Anyone who gets your private key file can use it immediately
- **With passphrase** — The key file is encrypted; the passphrase is needed to unlock it

> **Always use a passphrase** for keys used by humans. The SSH agent (covered later) lets you unlock the key once and use it without retyping the passphrase.

For automation (CI/CD, scripts), passphrase-less keys are sometimes used, but should be restricted in scope and carefully protected.
