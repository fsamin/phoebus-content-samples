---
title: "SSH Key Management"
type: lesson
estimated_duration: "20m"
---

# SSH Key Management

## How SSH Authentication Works

SSH supports two authentication methods:

1. **Password authentication** — simple but insecure (brute-forceable)
2. **Key-based authentication** — cryptographically strong, the standard for DevOps

Key-based auth uses **asymmetric cryptography**:
- **Private key** stays on your machine (never share it!)
- **Public key** goes on the remote server

When you connect, the server challenges your client to prove it holds the private key without ever transmitting it.

## Generating SSH Keys

### Ed25519 (Recommended)

```bash
ssh-keygen -t ed25519 -C "devops@company.com"
```

- **Ed25519** is modern, fast, and produces small keys
- `-C` adds a comment to identify the key

### RSA (Legacy Compatibility)

```bash
ssh-keygen -t rsa -b 4096 -C "devops@company.com"
```

- Use only when Ed25519 is not supported
- Always use 4096 bits minimum

## Key Files

```
~/.ssh/
├── id_ed25519          # Private key (chmod 600!)
├── id_ed25519.pub      # Public key (safe to share)
├── authorized_keys     # Public keys allowed to connect TO this machine
├── known_hosts         # Fingerprints of servers you've connected to
└── config              # SSH client configuration
```

## Deploying Your Public Key

### Method 1: ssh-copy-id

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@server
```

### Method 2: Manual

```bash
cat ~/.ssh/id_ed25519.pub | ssh user@server "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

### Method 3: Cloud providers

Most cloud providers (AWS, GCP, Azure) let you upload your public key during instance creation or via their CLI.

## Multiple Keys

As a DevOps engineer, you'll have different keys for different purposes:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/github_ed25519 -C "github"
ssh-keygen -t ed25519 -f ~/.ssh/work_ed25519 -C "work-servers"
ssh-keygen -t ed25519 -f ~/.ssh/personal_ed25519 -C "personal"
```

## Key Passphrases

Always protect your private keys with a passphrase. Use `ssh-agent` to avoid typing it repeatedly:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

## Security Best Practices

1. **Never share your private key** — generate a new one per machine
2. **Always use a passphrase** — protects against key theft
3. **Use Ed25519** — smaller, faster, more secure than RSA
4. **Rotate keys periodically** — at least annually
5. **Disable password auth** on servers once key auth is set up
