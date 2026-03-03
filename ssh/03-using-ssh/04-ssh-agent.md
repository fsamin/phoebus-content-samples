---
title: "SSH Agent"
type: lesson
estimated_duration: "10m"
---

# SSH Agent and Agent Forwarding

## The Problem

If your private key is protected with a passphrase (it should be!), you need to type it every time you use the key. This gets old fast when you connect to servers dozens of times a day.

## The SSH Agent

The SSH agent is a background process that **holds your unlocked private keys in memory**. Once you unlock a key, the agent handles authentication automatically for the rest of your session.

### Starting the Agent

**Linux**:
```bash
# Start the agent (usually done in your shell profile)
eval "$(ssh-agent -s)"
# Output: Agent pid 12345
```

**macOS**: The agent starts automatically.

**Windows**: Enable the ssh-agent service (see the Keys module).

### Adding Keys to the Agent

```bash
# Add your default key
ssh-add ~/.ssh/id_ed25519
# Enter passphrase once, then it's cached

# Add a specific key
ssh-add ~/.ssh/id_work

# List loaded keys
ssh-add -l

# Remove all keys
ssh-add -D
```

After adding, SSH connections use the agent automatically — no more passphrase prompts.

### Auto-Add on First Use

Add to `~/.ssh/config`:

```
Host *
    AddKeysToAgent yes
```

Now keys are automatically added to the agent the first time you use them.

## Agent Forwarding

Agent forwarding solves a specific problem: **you're on Server A and need to connect to Server B**, but your private key is on your laptop.

```
Without forwarding:
  Laptop → Server A → Server B
                       ❌ Your key isn't here!

With forwarding:
  Laptop → Server A → Server B
                       ✅ Agent on laptop handles auth
```

### How It Works

1. Your SSH agent runs on your laptop with your loaded keys
2. You connect to Server A with agent forwarding enabled
3. From Server A, you connect to Server B
4. Server B's authentication request is forwarded back through the chain to your laptop's agent
5. Your agent signs the challenge, and the response travels back

**Your private key never leaves your laptop.** Only the authentication challenge/response is forwarded.

### Enabling Agent Forwarding

```bash
# Command line
ssh -A user@server-a

# Or in ~/.ssh/config
Host server-a
    ForwardAgent yes
```

From Server A, you can now connect to Server B as if your keys were local:

```bash
# On Server A:
ssh user@server-b   # Works! Uses your laptop's agent
```

### Security Warning

> ⚠️ **Only enable agent forwarding to servers you trust.** A malicious administrator on the intermediate server could potentially use your forwarded agent to authenticate as you to other servers while your connection is active.

For production environments, **ProxyJump** (covered in the SSH config lesson) is a safer alternative because it creates a direct encrypted tunnel without giving the intermediate server access to your agent.

```
# Safer alternative: ProxyJump
Host internal-server
    ProxyJump bastion
    # No ForwardAgent needed — direct tunnel
```
