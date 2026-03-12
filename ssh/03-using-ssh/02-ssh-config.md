---
title: "SSH Config File"
type: lesson
estimated_duration: "12m"
---

# SSH Config File

Typing `ssh -i ~/.ssh/id_work -p 2222 deploy@long-server-name.internal.example.com` every time is painful. The SSH config file solves this.

## Location

- **Linux/macOS**: `~/.ssh/config`
- **Windows**: `C:\Users\YourName\.ssh\config`

Create it if it doesn't exist:

```bash
touch ~/.ssh/config
chmod 600 ~/.ssh/config
```

## Basic Syntax

```
Host nickname
    HostName actual.server.address.com
    User username
    Port 22
    IdentityFile ~/.ssh/id_ed25519
```

Now instead of the full command, you just type:

```bash
ssh nickname
```

## Practical Examples

### Multiple Servers

```
# Production web server
Host prod-web
    HostName 203.0.113.10
    User deploy
    Port 22
    IdentityFile ~/.ssh/id_work

# Staging server
Host staging
    HostName staging.example.com
    User admin
    Port 2222
    IdentityFile ~/.ssh/id_work

# Personal server
Host home
    HostName myserver.dyndns.org
    User pi
    Port 22
    IdentityFile ~/.ssh/id_personal
```

Usage:

```bash
ssh prod-web     # connects to 203.0.113.10 as deploy
ssh staging      # connects to staging.example.com:2222 as admin
ssh home         # connects to myserver.dyndns.org as pi
```

### Wildcard Patterns

Apply settings to groups of servers:

```
# All internal servers
Host *.internal.example.com
    User admin
    IdentityFile ~/.ssh/id_work
    ProxyJump bastion.example.com

# All servers: keep connections alive
Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
    AddKeysToAgent yes
```

### Jump Host (ProxyJump)

Access servers behind a firewall through an intermediate host:

```
# The bastion / jump host
Host bastion
    HostName bastion.example.com
    User admin

# Server behind the bastion
Host internal-db
    HostName 10.0.1.50
    User dbadmin
    ProxyJump bastion
```

Now `ssh internal-db` automatically connects through the bastion:

```mermaid
graph LR
    You["💻 Your machine"] --> Bastion["bastion.example.com"] --> Target["10.0.1.50"]
```

## Useful Options

| Option | Description | Example |
|--------|-------------|---------|
| `ServerAliveInterval` | Send keepalive every N seconds | `60` |
| `ServerAliveCountMax` | Close after N missed keepalives | `3` |
| `ConnectTimeout` | Timeout in seconds | `10` |
| `ForwardAgent` | Forward your SSH agent | `yes` |
| `LocalForward` | Set up a tunnel | `5432 localhost:5432` |
| `Compression` | Compress data (slow links) | `yes` |

## Priority Rules

SSH config is processed top-to-bottom. **The first matching value wins**. Put specific hosts before wildcards:

```
# ✅ Correct order
Host prod-web
    User deploy

Host *
    User admin

# ❌ Wrong order — prod-web would get User=admin
Host *
    User admin

Host prod-web
    User deploy
```
