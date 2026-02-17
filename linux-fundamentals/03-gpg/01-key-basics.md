---
title: "GPG Key Basics"
type: lesson
estimated_duration: "20m"
---

# GPG Key Basics

## What is GPG?

GPG (GNU Privacy Guard) is an implementation of the OpenPGP standard. In DevOps, it's used for:

- **Signing Git commits** — prove you authored the code
- **Encrypting secrets** — protect sensitive data at rest
- **Verifying packages** — ensure software authenticity (apt, rpm)
- **Signing artifacts** — Docker images, Helm charts, binaries

## Key Concepts

### Key Pairs

Like SSH, GPG uses asymmetric cryptography:
- **Public key** — shared with others; used to encrypt messages TO you and verify signatures FROM you
- **Private key** — kept secret; used to decrypt messages and create signatures

### Key ID and Fingerprint

```
pub   ed25519 2024-01-15 [SC] [expires: 2026-01-15]
      AB12 CD34 EF56 7890 1234  5678 9ABC DEF0 1234 5678
uid           [ultimate] DevOps Engineer <devops@company.com>
sub   cv25519 2024-01-15 [E] [expires: 2026-01-15]
```

- **Fingerprint**: the full 40-character hex string
- **Key ID**: the last 8 (short) or 16 (long) characters
- **`[SC]`**: Sign + Certify capabilities
- **`[E]`**: Encryption capability (on the subkey)

## Generating a GPG Key

```bash
gpg --full-generate-key
```

Choose:
1. **Algorithm**: Ed25519 (option 9 or 10)
2. **Expiration**: 2 years (always set an expiration)
3. **Real name and email**: must match your Git config

### Quick generation

```bash
gpg --quick-generate-key "DevOps Engineer <devops@company.com>" ed25519 sign 2y
```

## Listing Keys

```bash
# List public keys
gpg --list-keys
gpg -k

# List secret (private) keys
gpg --list-secret-keys
gpg -K

# Show key with fingerprint
gpg --list-keys --keyid-format long
```

## Exporting Keys

```bash
# Export public key (to share)
gpg --armor --export devops@company.com > public.asc

# Export private key (for backup)
gpg --armor --export-secret-keys devops@company.com > private.asc
```

> ⚠️ **Never share your private key.** Store the backup in a secure location (password manager, encrypted USB drive).

## Importing Keys

```bash
# Import a public key
gpg --import colleague-public.asc

# Import from a keyserver
gpg --keyserver keys.openpgp.org --recv-keys AB12CD34EF567890
```

## Summary

GPG keys are your digital identity in the DevOps world. They prove authorship of code, protect secrets, and verify that software hasn't been tampered with. Always use Ed25519, always set an expiration date.
