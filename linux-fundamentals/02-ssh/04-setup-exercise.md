---
title: "Set Up SSH Access"
type: terminal-exercise
estimated_duration: "10m"
---

# Set Up SSH Access

You need to set up SSH key authentication for a new production server.

## Step 1: Generate a new SSH key pair

Generate an Ed25519 key with a meaningful comment.

```console
$ ▌
```

- [x] `ssh-keygen -t ed25519 -C "prod-server"` — Creates a modern Ed25519 key pair with a descriptive comment.
- [ ] `ssh-keygen` — Uses default RSA which is less secure and creates larger keys.
- [ ] `ssh-keygen -t dsa` — DSA is deprecated and should never be used.

```output
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/devops/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/devops/.ssh/id_ed25519
Your public key has been saved in /home/devops/.ssh/id_ed25519.pub
```

## Step 2: Set correct permissions on the private key

SSH will refuse to use a private key with loose permissions. Fix it.

```console
$ ▌
```

- [ ] `chmod 644 ~/.ssh/id_ed25519` — Too permissive! Group and others can read your private key.
- [x] `chmod 600 ~/.ssh/id_ed25519` — Only the owner can read and write. This is what SSH requires.
- [ ] `chmod 777 ~/.ssh/id_ed25519` — Never do this! Everyone would have full access to your key.

```output
```

## Step 3: Deploy the public key to the remote server

Copy your public key to the production server.

```console
$ ▌
```

- [x] `ssh-copy-id -i ~/.ssh/id_ed25519.pub devops@prod-server` — Securely copies the public key and sets correct permissions on the remote.
- [ ] `scp ~/.ssh/id_ed25519 devops@prod-server:~/.ssh/` — NEVER copy your private key to a remote server!
- [ ] `cat ~/.ssh/id_ed25519.pub` — Displays the key but doesn't deploy it.

```output
Number of key(s) added: 1
Now try logging into the machine, with: "ssh devops@prod-server"
```
