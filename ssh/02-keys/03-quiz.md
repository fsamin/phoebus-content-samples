---
title: "SSH Keys Quiz"
type: quiz
estimated_duration: "8m"
---

# SSH Keys Quiz

## [multiple-choice] Which SSH key algorithm is recommended for new keys?

- [ ] RSA with 2048 bits
- [ ] DSA
- [x] Ed25519
- [ ] ECDSA with 256 bits

> **Explanation:** Ed25519 is the modern standard: it produces the smallest keys, is the fastest algorithm, and has excellent security. It's the best choice unless you need compatibility with very old systems.

## [multiple-choice] What does the passphrase protect?

- [ ] The server from unauthorized access
- [x] The private key file on disk — encrypting it so it can't be used if stolen
- [ ] The public key from being modified
- [ ] The SSH connection from being intercepted

> **Explanation:** The passphrase encrypts your private key file. Without it, anyone who gets a copy of the file can use it. With a passphrase, they would also need to know the passphrase.

## [short-answer] What file permissions should the private key file have on Linux?

    600

> **Explanation:** The private key file must be `600` (read/write by owner only). SSH will refuse to use a key file with permissions that are too open, as it could indicate the key has been compromised.

## [multiple-choice] What is the `authorized_keys` file?

- [ ] A file containing the server's private keys
- [ ] A file listing blocked IP addresses
- [x] A file on the server listing public keys that are allowed to authenticate
- [ ] A file containing encrypted passwords

> **Explanation:** `~/.ssh/authorized_keys` on the server contains one public key per line. When you connect, the server checks if your private key matches any of the public keys in this file.

## [multiple-choice] How do you copy your public key to a server on Linux/macOS?

- [ ] `scp ~/.ssh/id_ed25519 user@server:`
- [ ] `ssh-keygen --deploy user@server`
- [x] `ssh-copy-id -i ~/.ssh/id_ed25519.pub user@server`
- [ ] `ssh user@server --install-key`

> **Explanation:** `ssh-copy-id` is the standard tool. It connects to the server via SSH (using your password the first time), then appends your public key to the server's `authorized_keys` file with correct permissions.

## [multiple-choice] On modern Windows 10/11, how do you generate an SSH key?

- [ ] You must install PuTTY
- [ ] You must download OpenSSH separately
- [x] Open PowerShell and run `ssh-keygen -t ed25519`
- [ ] Use the Windows Key Manager in Control Panel

> **Explanation:** Windows 10+ includes OpenSSH as a built-in feature. You can use `ssh-keygen` directly in PowerShell, just like on Linux and macOS. PuTTY is no longer necessary.

## [multiple-choice] Where does macOS store the SSH passphrase when using `--apple-use-keychain`?

- [ ] In the `~/.ssh/config` file
- [x] In the macOS Keychain, so it persists across reboots
- [ ] In the private key file itself
- [ ] In the SSH agent only (lost on reboot)

> **Explanation:** The `--apple-use-keychain` flag stores the passphrase in the macOS Keychain. Combined with `UseKeychain yes` in `~/.ssh/config`, the key is automatically unlocked after login without retyping the passphrase.
