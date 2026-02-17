---
title: "SSH Quiz"
type: quiz
estimated_duration: "10m"
---

# SSH Quiz

## [multiple-choice] Which SSH key type is recommended for new deployments?

- [ ] RSA 2048
- [ ] DSA
- [x] Ed25519
- [ ] ECDSA

> **Explanation:** Ed25519 provides excellent security with small key sizes and fast operations. RSA 2048 is too weak, DSA is deprecated, and ECDSA has potential implementation concerns.

## [multiple-choice] What permissions should an SSH private key have?

- [ ] 644
- [ ] 755
- [x] 600
- [ ] 400

> **Explanation:** `600` (rw-------) means only the owner can read and write. SSH will refuse to use a key with permissions more permissive than this. `400` (r--------) also works but `600` is the standard.

## [short-answer] What SSH command creates a local port forward from local port 3000 to remote port 5432 on host "dbserver"?

    ssh -L 3000:localhost:5432 dbserver

> **Explanation:** `-L local_port:target_host:target_port` creates a local port forward. Traffic to `localhost:3000` will be tunneled through SSH and sent to `localhost:5432` on the dbserver.

## [multiple-choice] What does `ProxyJump` in SSH config do?

- [ ] Starts a SOCKS proxy
- [x] Connects through an intermediate (jump) host
- [ ] Enables TCP port forwarding
- [ ] Proxies HTTP traffic

> **Explanation:** `ProxyJump` specifies one or more intermediate hosts to connect through. This is the modern replacement for `ProxyCommand ssh -W %h:%p bastion`.

## [multiple-choice] Which file on the remote server stores authorized public keys?

- [ ] `~/.ssh/known_hosts`
- [ ] `~/.ssh/config`
- [x] `~/.ssh/authorized_keys`
- [ ] `~/.ssh/id_ed25519.pub`

> **Explanation:** `authorized_keys` contains one public key per line. When you connect, the server checks if your key matches one in this file. `known_hosts` is for server fingerprints, and `config` is for client settings.
