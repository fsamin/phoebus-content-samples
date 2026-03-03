---
title: "Connecting to Servers"
type: lesson
estimated_duration: "12m"
---

# Connecting to Servers

## Your First SSH Connection

The basic syntax:

```bash
ssh user@hostname
```

Example:

```bash
ssh admin@192.168.1.100
# or with a domain name
ssh deploy@web-server.example.com
```

### First Connection: Host Key Verification

The very first time you connect to a server, you'll see this message:

```
The authenticity of host 'server.example.com (203.0.113.42)' can't be established.
ED25519 key fingerprint is SHA256:xR4jKs9B2qF...
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'server.example.com' (ED25519) to the list of known hosts.
```

This is SSH asking: "I've never seen this server before. Do you trust it?" When you say `yes`, the server's public key is saved in `~/.ssh/known_hosts`. Future connections verify the server's identity automatically.

> ⚠️ **If you see a warning about a changed host key**, do NOT ignore it. It could mean someone is intercepting your connection (man-in-the-middle attack), or the server was reinstalled.

### Common Connection Options

```bash
# Connect on a non-standard port
ssh -p 2222 user@server

# Use a specific key
ssh -i ~/.ssh/id_work user@server

# Verbose mode (for debugging)
ssh -v user@server     # one level
ssh -vv user@server    # more detail
ssh -vvv user@server   # maximum detail
```

## Running Remote Commands

You don't have to open an interactive shell. You can run a single command:

```bash
# Check disk space on the server
ssh user@server df -h

# Check who is logged in
ssh user@server who

# Read a log file
ssh user@server tail -100 /var/log/syslog
```

The command runs on the remote machine, output is displayed locally, and the connection closes automatically.

### Chaining Commands

```bash
ssh user@server 'cd /opt/app && git pull && systemctl restart app'
```

Use quotes to prevent your local shell from interpreting `&&`.

## Interactive vs Non-Interactive Sessions

| Type | Use Case | Example |
|------|----------|---------|
| **Interactive** | Exploring, debugging, manual tasks | `ssh user@server` |
| **Non-interactive** | Scripts, automation, CI/CD | `ssh user@server uptime` |

For non-interactive sessions with commands that need a terminal (like `top` or `vim`), force a TTY:

```bash
ssh -t user@server top
```

## Connection Timeout

If a server is unreachable, SSH can hang for a long time. Set a timeout:

```bash
# Timeout after 5 seconds if no connection
ssh -o ConnectTimeout=5 user@server
```
