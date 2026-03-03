---
title: "Known Hosts and Host Key Verification"
type: lesson
estimated_duration: "10m"
---

# Known Hosts and Host Key Verification

## The `known_hosts` File

Every time you accept a server's fingerprint, SSH stores it in `~/.ssh/known_hosts`:

```
server.example.com ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBqj...
192.168.1.100 ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAAC...
```

Each line maps a hostname (or IP) to the server's public key. On future connections, SSH compares the server's key to this stored value.

## The Man-in-the-Middle Warning

If a server's key changes, SSH shows a scary warning:

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
```

This can mean:
1. **The server was reinstalled** — New OS = new host keys. Common and benign.
2. **The server's SSH was reconfigured** — Admin changed key types.
3. **An actual attack** — Someone is intercepting your connection. Rare but dangerous.

### How to Handle It

**If you expect the change** (server reinstall, migration):

```bash
# Remove the old key
ssh-keygen -R server.example.com

# Then connect again — you'll be prompted to accept the new key
ssh user@server.example.com
```

**If you don't expect it**: contact the server administrator before connecting. Don't blindly remove the key.

## Hashed Known Hosts

Some systems hash hostnames in `known_hosts` for privacy:

```
|1|abc123...=|def456...= ssh-ed25519 AAAAC3NzaC1lZDI1NTE5...
```

This way, if someone reads your `known_hosts`, they can't see which servers you connect to. Enable globally:

```
Host *
    HashKnownHosts yes
```

## Strict Host Key Checking

Control SSH's behavior when a host key is unknown:

| Setting | Behavior |
|---------|----------|
| `StrictHostKeyChecking ask` | Prompt the user (default) |
| `StrictHostKeyChecking yes` | Refuse to connect if key is unknown |
| `StrictHostKeyChecking no` | Accept any key without asking |

```
# For automated scripts connecting to known servers
Host known-server
    StrictHostKeyChecking yes

# NEVER use "no" in production — only for ephemeral test environments
```
