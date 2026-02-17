---
title: "SSH Config & Productivity"
type: lesson
estimated_duration: "15m"
---

# SSH Config & Productivity

## The SSH Config File

The file `~/.ssh/config` is your SSH power tool. It lets you define aliases, default settings, and per-host configurations.

## Basic Configuration

```
# Default settings for all hosts
Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
    AddKeysToAgent yes

# Production jump host
Host bastion
    HostName bastion.prod.company.com
    User devops
    IdentityFile ~/.ssh/work_ed25519
    Port 22

# Production web servers (via bastion)
Host prod-web-*
    User deploy
    ProxyJump bastion
    IdentityFile ~/.ssh/work_ed25519

# GitHub
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/github_ed25519

# Staging environment
Host staging
    HostName 10.0.1.50
    User admin
    IdentityFile ~/.ssh/work_ed25519
    LocalForward 3000 localhost:3000
```

## Key Directives

| Directive | Purpose |
|-----------|---------|
| `HostName` | Real hostname or IP |
| `User` | Default username |
| `Port` | SSH port (default: 22) |
| `IdentityFile` | Which private key to use |
| `ProxyJump` | Connect through a jump host |
| `LocalForward` | Forward local port to remote |
| `RemoteForward` | Forward remote port to local |
| `ServerAliveInterval` | Keep connection alive |
| `ForwardAgent` | Forward SSH agent (use carefully!) |

## Usage

With the config above:

```bash
# Instead of:
ssh -i ~/.ssh/work_ed25519 -p 22 devops@bastion.prod.company.com

# Just type:
ssh bastion

# Connect to any production web server through bastion:
ssh prod-web-01

# Clone from GitHub using the right key automatically:
git clone git@github.com:company/repo.git
```

## Multiplexing

Speed up repeated connections to the same host:

```
Host *
    ControlMaster auto
    ControlPath ~/.ssh/sockets/%r@%h-%p
    ControlPersist 600
```

The first connection opens a socket. Subsequent connections reuse it — instant login.

```bash
mkdir -p ~/.ssh/sockets
```

## Summary

A well-crafted SSH config file transforms your daily workflow. You type `ssh prod` instead of remembering hostnames, ports, and key paths.
