---
title: "File Transfers"
type: lesson
estimated_duration: "10m"
---

# File Transfers with SSH

SSH provides several tools for transferring files securely between machines.

## SCP — Secure Copy

The simplest way to copy files over SSH. Think of it as `cp` over the network.

### Copy a File to a Server

```bash
scp file.txt user@server:/home/user/
# or to a specific path
scp report.pdf user@server:/tmp/
```

### Copy a File from a Server

```bash
scp user@server:/var/log/app.log ./
```

### Copy a Directory (Recursive)

```bash
scp -r ./my-project/ user@server:/opt/
```

### Useful Options

```bash
# Use a specific port
scp -P 2222 file.txt user@server:/tmp/

# Use a specific key
scp -i ~/.ssh/id_work file.txt user@server:/tmp/

# Show progress
scp -v file.txt user@server:/tmp/
```

> 💡 Note the uppercase `-P` for port in scp (vs lowercase `-p` in ssh).

## SFTP — Secure File Transfer Protocol

SFTP provides an interactive file transfer session, like a remote file explorer in the terminal.

```bash
sftp user@server
```

You get an `sftp>` prompt with commands:

```
sftp> ls                    # List remote files
sftp> cd /var/log           # Change remote directory
sftp> lcd ~/downloads       # Change local directory
sftp> get app.log           # Download a file
sftp> put config.yaml       # Upload a file
sftp> mget *.log            # Download multiple files
sftp> mkdir backups         # Create remote directory
sftp> exit
```

SFTP is useful when you need to browse remote files before deciding what to transfer.

## Rsync — Smart Synchronization

Rsync is the most powerful option. It only transfers **what has changed**, making it much faster for repeated syncs.

```bash
# Sync a directory to the server
rsync -avz ./project/ user@server:/opt/project/
```

Flags explained:
- `-a` — Archive mode (preserves permissions, timestamps, symlinks)
- `-v` — Verbose output
- `-z` — Compress data during transfer

### Common Patterns

```bash
# Sync and delete files that no longer exist locally
rsync -avz --delete ./project/ user@server:/opt/project/

# Dry run — see what would happen without actually doing it
rsync -avzn ./project/ user@server:/opt/project/

# Exclude certain files
rsync -avz --exclude='node_modules' --exclude='.git' \
    ./project/ user@server:/opt/project/

# Use a specific SSH port
rsync -avz -e 'ssh -p 2222' ./project/ user@server:/opt/project/
```

### Why Rsync is Great

| Feature | SCP | Rsync |
|---------|-----|-------|
| Transfers everything each time | ✅ | ❌ Only changes |
| Preserves permissions | ❌ | ✅ |
| Compression | ❌ | ✅ |
| Resume interrupted transfers | ❌ | ✅ |
| Delete remote files | ❌ | ✅ |
| Exclude patterns | ❌ | ✅ |

> 💡 **Tip**: For one-off file copies, `scp` is fine. For regular synchronization (backups, deployments), always use `rsync`.
