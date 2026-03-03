---
title: "Understanding SSH"
type: lesson
estimated_duration: "15m"
---

# Understanding SSH

## The Problem: Remote Access Before SSH

Before SSH, system administrators used **Telnet** to connect to remote servers. Telnet worked, but it had a fatal flaw: **everything was sent in plain text** — including your username and password.

```
Telnet (before SSH):
  Admin ──── "password123" ────► Server
                  │
             Anyone on the
             network can read
             your password
```

In the early days of the internet, this wasn't considered a major issue because networks were small and trusted. But as the internet grew, this became a serious security risk.

## The Birth of SSH

In 1995, Finnish researcher **Tatu Ylönen** had his university network compromised by a password-sniffing attack. He wrote SSH (Secure Shell) as a replacement for Telnet that **encrypts everything**.

```
SSH:
  Admin ──── [encrypted data] ────► Server
                    │
               Attacker sees only
               meaningless gibberish
```

SSH quickly became the standard for remote server access. Today, virtually every server in the world runs an SSH daemon.

## What SSH Does

SSH provides three fundamental services:

### 1. Encrypted Remote Shell

The primary use: get a command-line terminal on a remote machine.

```bash
ssh user@server.example.com
# You now have a shell on the remote machine
```

Everything you type and everything the server responds is encrypted.

### 2. Secure File Transfer

Transfer files between machines:

```bash
# Copy a file to the server
scp file.txt user@server:/home/user/

# Copy a file from the server
scp user@server:/var/log/app.log ./

# Sync entire directories
rsync -avz ./project/ user@server:/opt/project/
```

### 3. Secure Tunneling

Forward network traffic through an encrypted SSH tunnel:

```bash
# Access a remote database as if it were local
ssh -L 5432:localhost:5432 user@server
# Now connect to localhost:5432 to reach the server's PostgreSQL
```

## How SSH Works (Simplified)

When you connect to an SSH server, several things happen:

```
1. TCP Connection
   Client ──── TCP port 22 ────► Server

2. Protocol Negotiation
   "I support SSH-2.0"  ◄────►  "Me too, let's use SSH-2.0"

3. Key Exchange (Encryption Setup)
   Client and Server agree on encryption keys
   (using Diffie-Hellman or similar)
   → All communication is now encrypted

4. Server Authentication
   Server proves its identity with its host key
   Client checks: "Is this really the server I expect?"

5. User Authentication
   Client proves its identity:
   → Option A: Password
   → Option B: SSH key (public key authentication)

6. Session
   Encrypted shell session begins
```

The critical insight: **encryption is established before any credentials are sent**. Even if you use password authentication, the password is encrypted in transit.

## SSH Components

| Component | Role | Default Port |
|-----------|------|-------------|
| **ssh** | The client — you run this to connect | — |
| **sshd** | The server daemon — runs on the remote machine | 22 |
| **ssh-keygen** | Generates SSH key pairs | — |
| **ssh-agent** | Caches your keys in memory | — |
| **scp** | Secure file copy | — |
| **sftp** | Secure FTP-like file transfer | — |

## SSH Versions

| Version | Status |
|---------|--------|
| SSH-1 | **Deprecated and insecure** — never use |
| SSH-2 | Current standard — all modern implementations |

The most widely used SSH implementation is **OpenSSH**, maintained by the OpenBSD project. It's included by default on Linux, macOS, and Windows 10+.
