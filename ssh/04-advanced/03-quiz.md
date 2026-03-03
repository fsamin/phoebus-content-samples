---
title: "SSH Security Quiz"
type: quiz
estimated_duration: "8m"
---

# SSH Security Quiz

## [multiple-choice] What does the `known_hosts` file contain?

- [ ] A list of users allowed to connect
- [ ] Your private keys
- [x] The public keys of servers you've connected to before
- [ ] A log of all SSH connections

> **Explanation:** `~/.ssh/known_hosts` stores the public key of every server you've connected to. SSH uses this to verify that the server hasn't changed (protection against man-in-the-middle attacks).

## [multiple-choice] What should you do if SSH warns that a host key has changed unexpectedly?

- [ ] Delete the `known_hosts` file entirely
- [ ] Add `StrictHostKeyChecking no` to your config
- [x] Contact the server administrator to confirm the change before connecting
- [ ] Ignore the warning and connect anyway

> **Explanation:** An unexpected host key change could indicate a man-in-the-middle attack. Always verify with the server admin before accepting the new key. If they confirm a reinstall or reconfiguration, then use `ssh-keygen -R hostname` to remove the old key.

## [multiple-choice] Which `sshd_config` setting should be changed FIRST to secure a server?

- [ ] Change the port to 2222
- [x] Set `PasswordAuthentication no` (after confirming key auth works)
- [ ] Install Fail2Ban
- [ ] Set `ClientAliveInterval`

> **Explanation:** Disabling password authentication eliminates the most common attack vector: brute-force password guessing. This should be done as soon as key-based authentication is confirmed working.

## [multiple-choice] Why should `PermitRootLogin` be set to `no`?

- [ ] Root accounts don't support SSH keys
- [ ] Root connections are not encrypted
- [x] It prevents direct root access, forcing administrators to use named accounts with sudo for accountability
- [ ] It makes SSH faster

> **Explanation:** Direct root login hides who performed actions. With `PermitRootLogin no`, each admin logs in as themselves and uses `sudo`, creating clear audit trails of who did what.

## [short-answer] What command tests sshd_config for syntax errors before restarting?

    sudo sshd -t

> **Explanation:** Always run `sshd -t` before restarting the SSH service. A syntax error in `sshd_config` could prevent sshd from starting, locking you out of the server.

## [multiple-choice] What does Fail2Ban do?

- [ ] Encrypts SSH traffic with additional algorithms
- [ ] Monitors SSH performance
- [x] Automatically bans IP addresses after too many failed authentication attempts
- [ ] Generates SSH keys for new users

> **Explanation:** Fail2Ban watches authentication logs and uses firewall rules (iptables/nftables) to temporarily ban IPs that fail too many times. This stops brute-force attacks from consuming resources.
