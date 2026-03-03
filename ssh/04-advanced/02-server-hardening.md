---
title: "Server Hardening"
type: lesson
estimated_duration: "12m"
---

# SSH Server Hardening

Securing your SSH server is one of the most important steps in system administration. The configuration file is `/etc/ssh/sshd_config`.

## Essential Security Settings

### 1. Disable Password Authentication

Once key authentication works, disable passwords:

```
# /etc/ssh/sshd_config
PasswordAuthentication no
ChallengeResponseAuthentication no
```

This eliminates brute-force password attacks entirely.

### 2. Disable Root Login

Never allow direct SSH login as root:

```
PermitRootLogin no
```

Administrators should log in as their own user and use `sudo` for elevated tasks. This provides audit trails (who did what).

If root login is absolutely needed for automation, restrict it to keys only:

```
PermitRootLogin prohibit-password
```

### 3. Limit Users and Groups

Only allow specific users or groups to connect:

```
AllowUsers deploy admin
# or
AllowGroups ssh-users
```

Anyone not listed is denied, even with valid credentials.

### 4. Change the Default Port

Moving SSH to a non-standard port reduces automated scans (but is **not** real security — it's "security through obscurity"):

```
Port 2222
```

> ⚠️ Make sure to update firewall rules and your SSH config before changing the port, or you'll lock yourself out!

### 5. Use Only SSH Protocol 2

Ensure the deprecated SSHv1 is disabled:

```
Protocol 2
```

### 6. Set Idle Timeout

Disconnect inactive sessions:

```
ClientAliveInterval 300    # Send keepalive every 5 minutes
ClientAliveCountMax 2      # Disconnect after 2 missed responses
```

This closes forgotten sessions after 10 minutes of inactivity.

### 7. Limit Authentication Attempts

```
MaxAuthTries 3
```

After 3 failed attempts, the connection is dropped.

## Applying Changes

After editing `/etc/ssh/sshd_config`, restart the SSH service:

```bash
# Test the config first (catch syntax errors)
sudo sshd -t

# Restart the service
sudo systemctl restart sshd
```

> 💡 **Pro tip**: Always keep an existing SSH session open while testing configuration changes. If you lock yourself out, the existing session can fix it.

## Additional Protection: Fail2Ban

Fail2Ban monitors SSH logs and automatically bans IPs that fail authentication too many times:

```bash
sudo apt install fail2ban

# Default config bans IPs for 10 minutes after 5 failed attempts
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

Check banned IPs:
```bash
sudo fail2ban-client status sshd
```

## Security Checklist

| Setting | Value | Priority |
|---------|-------|----------|
| `PasswordAuthentication` | `no` | 🔴 Critical |
| `PermitRootLogin` | `no` | 🔴 Critical |
| `Protocol` | `2` | 🔴 Critical |
| `AllowUsers` or `AllowGroups` | Set | 🟠 Important |
| `MaxAuthTries` | `3` | 🟠 Important |
| `ClientAliveInterval` | `300` | 🟡 Recommended |
| Non-standard port | `2222` etc. | 🟢 Optional |
| Fail2Ban installed | Yes | 🟠 Important |
