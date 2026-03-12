---
title: "Using The Bastion"
type: lesson
estimated_duration: "20m"
---

# Using The Bastion as a User

This lesson covers the day-to-day usage of The Bastion from a user's perspective. Your bastion administrator will have set up your account; here's how to use it.

## Connecting to The Bastion

First, connect to the bastion itself:

```bash
ssh user@bastion.example.com
```

This opens The Bastion's interactive shell, where you can run management commands (called **osh commands**).

## Essential Osh Commands

All bastion commands start with `--osh`. Here are the most important ones:

### Check Your Account

```bash
ssh user@bastion.example.com --osh info
```

This shows your account status, registered keys, and group memberships.

### List Your Keys

```bash
# List your ingress keys (keys you use to log in to the bastion)
ssh user@bastion.example.com --osh selfListIngressKeys

# List your egress keys (keys the bastion uses to connect to servers)
ssh user@bastion.example.com --osh selfListEgressKeys
```

### Generate an Egress Key

Your egress key is what the bastion uses to connect to target servers. Generate one if you don't have one:

```bash
ssh user@bastion.example.com --osh selfGenerateEgressKey --algo ed25519 --size 256
```

You'll get a public key to install on your target servers (or give to the server admin).

### View Your Accesses

```bash
# List servers you can access personally
ssh user@bastion.example.com --osh selfListAccesses

# List servers accessible through your groups
ssh user@bastion.example.com --osh groupListServers --group my-team
```

## Connecting Through The Bastion

### Method 1: Direct Syntax (Most Common)

```bash
ssh user@bastion.example.com -- admin@target-server.internal
```

The `--` separates the bastion connection from the target. Everything after `--` specifies where you want to go.

```bash
# Connect as root to 10.0.1.50
ssh user@bastion.example.com -- root@10.0.1.50

# Connect with a specific port
ssh user@bastion.example.com -- admin@10.0.1.50 -p 2222
```

### Method 2: Interactive Mode

```bash
ssh user@bastion.example.com -i
```

This opens an interactive prompt where you can type commands:

```
bastion> connect admin@10.0.1.50
bastion> help
bastion> exit
```

### SSH Config Shortcut

To avoid typing the bastion address every time, add to `~/.ssh/config`:

```
# Direct connection through the bastion
Host *.internal
    ProxyCommand ssh user@bastion.example.com -- %r@%h -p %p

# Or a specific server
Host prod-db
    ProxyCommand ssh user@bastion.example.com -- dbadmin@10.0.1.50
```

Now you can simply:

```bash
ssh prod-db
```

## Managing Personal Access

If you have permission to manage your own accesses:

```bash
# Request access to a server
ssh user@bastion.example.com --osh selfAddPersonalAccess \
    --host 10.0.1.50 --user admin --port 22

# Remove an access
ssh user@bastion.example.com --osh selfDelPersonalAccess \
    --host 10.0.1.50 --user admin --port 22
```

> Note: Depending on the bastion configuration, personal access requests might require approval from a gate keeper.

## File Transfers Through The Bastion

The Bastion supports SCP, SFTP, and rsync:

### SCP

```bash
# Upload a file
scp -o "ProxyCommand ssh user@bastion.example.com -- %r@%h -p %p" \
    file.txt admin@target-server:/tmp/

# Download a file
scp -o "ProxyCommand ssh user@bastion.example.com -- %r@%h -p %p" \
    admin@target-server:/var/log/app.log ./
```

### With SSH Config

If you configured ProxyCommand in `~/.ssh/config`, it's simpler:

```bash
scp file.txt prod-db:/tmp/
scp prod-db:/var/log/app.log ./
```

## Daily Workflow Summary

```
Morning:
  1. ssh-add ~/.ssh/id_ed25519          # Unlock your key
  
Working:
  2. ssh me@bastion -- admin@server1    # Connect to servers as needed
  3. scp file.txt server1:/tmp/         # Transfer files (with config)
  
Checking:
  4. ssh me@bastion --osh selfListAccesses  # Review your accesses
  5. ssh me@bastion --osh info              # Check account status
```

## Common Troubleshooting

| Problem | Solution |
|---------|----------|
| "Access denied" | Check your accesses with `selfListAccesses` |
| "Key not found" | Verify egress key with `selfListEgressKeys` |
| "Connection timeout" | Check if target server allows bastion IP |
| "Permission denied (publickey)" | Your egress key isn't installed on the target |
