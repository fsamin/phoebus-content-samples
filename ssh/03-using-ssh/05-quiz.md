---
title: "Using SSH Quiz"
type: quiz
estimated_duration: "8m"
---

# Using SSH Quiz

## [multiple-choice] What happens the first time you connect to a new SSH server?

- [ ] SSH refuses the connection for security
- [ ] The server sends you its private key
- [x] SSH shows the server's key fingerprint and asks you to verify it
- [ ] SSH automatically trusts the server

> **Explanation:** On first connection, SSH displays the server's public key fingerprint and asks you to confirm. If you accept, the key is saved in `~/.ssh/known_hosts` for future verification.

## [short-answer] What file stores SSH connection shortcuts (aliases, ports, keys)?

    ~/.ssh/config

> **Explanation:** The `~/.ssh/config` file lets you define Host entries with aliases, custom ports, specific keys, and other options. It avoids typing long commands.

## [multiple-choice] In `~/.ssh/config`, if a `Host *` block appears before a `Host prod` block, which User value applies to `ssh prod`?

- [x] The User from `Host *` because the first match wins
- [ ] The User from `Host prod` because specific hosts take priority
- [ ] SSH merges both values
- [ ] SSH prompts you to choose

> **Explanation:** SSH config is processed top-to-bottom, and the first matching value for each option wins. Specific hosts should appear before wildcard (`*`) entries.

## [multiple-choice] What is the main advantage of rsync over scp?

- [ ] Rsync is encrypted while scp is not
- [ ] Rsync supports IPv6
- [x] Rsync only transfers files that have changed, making subsequent syncs much faster
- [ ] Rsync doesn't require SSH

> **Explanation:** Rsync uses a delta-transfer algorithm: it compares source and destination, then only sends the differences. For repeated transfers (backups, deployments), this is dramatically faster than scp which copies everything.

## [multiple-choice] What does the SSH agent do?

- [ ] Monitors server logs for security threats
- [ ] Automatically rotates your SSH keys
- [x] Holds your unlocked private keys in memory so you don't retype passphrases
- [ ] Encrypts your files on disk

> **Explanation:** The SSH agent is a background process that caches your decrypted private keys. After unlocking a key once with `ssh-add`, the agent handles authentication without asking for the passphrase again.

## [multiple-choice] Why should you be careful with SSH agent forwarding?

- [ ] It sends your private key to the remote server
- [ ] It disables encryption on the connection
- [x] A malicious admin on the intermediate server could use your forwarded agent to connect to other servers as you
- [ ] It makes connections slower

> **Explanation:** Agent forwarding doesn't send your key, but it does allow the intermediate server to request your agent to sign authentication challenges. A malicious admin could abuse this while your session is active. ProxyJump is a safer alternative.
