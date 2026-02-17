---
title: "Signing & Encryption"
type: lesson
estimated_duration: "15m"
---

# Signing & Encryption

## Signing Git Commits

The most common DevOps use of GPG is signing Git commits to prove authorship.

### Configure Git to use your GPG key

```bash
# Get your key ID
gpg --list-secret-keys --keyid-format long
# Look for: sec   ed25519/ABCDEF1234567890

# Configure Git
git config --global user.signingkey ABCDEF1234567890
git config --global commit.gpgsign true
git config --global tag.gpgsign true
```

### Sign commits

```bash
# Sign a single commit
git commit -S -m "feat: add new feature"

# With gpgsign=true, all commits are signed automatically
git commit -m "feat: add new feature"  # automatically signed

# Verify commit signatures
git log --show-signature
```

### Upload your public key to GitHub/GitLab

```bash
# Copy your public key
gpg --armor --export ABCDEF1234567890 | pbcopy  # macOS
gpg --armor --export ABCDEF1234567890 | xclip    # Linux
```

Paste it in GitHub → Settings → SSH and GPG keys → New GPG key.

## Encrypting Files

### Encrypt for a recipient

```bash
# Encrypt a file for a specific recipient
gpg --encrypt --recipient devops@company.com secrets.yaml
# Creates secrets.yaml.gpg

# Encrypt for multiple recipients
gpg --encrypt -r alice@company.com -r bob@company.com secrets.yaml
```

### Decrypt

```bash
gpg --decrypt secrets.yaml.gpg > secrets.yaml
```

### Symmetric encryption (password-based)

```bash
# Encrypt with a passphrase
gpg --symmetric --cipher-algo AES256 backup.tar.gz
# Creates backup.tar.gz.gpg

# Decrypt
gpg --decrypt backup.tar.gz.gpg > backup.tar.gz
```

## Signing Files

### Sign a file (detached signature)

```bash
gpg --detach-sign --armor release-v1.0.tar.gz
# Creates release-v1.0.tar.gz.asc
```

### Verify a signature

```bash
gpg --verify release-v1.0.tar.gz.asc release-v1.0.tar.gz
```

## DevOps Use Cases

| Use Case | Command |
|----------|---------|
| Sign Git commits | `git commit -S` |
| Encrypt secrets for deployment | `gpg --encrypt -r ops-team@company.com .env` |
| Sign release artifacts | `gpg --detach-sign binary` |
| Verify downloaded tools | `gpg --verify tool.asc tool` |
| Encrypt database backups | `gpg --symmetric --cipher-algo AES256 backup.sql` |

## Summary

Signing proves identity and integrity. Encryption protects confidentiality. In a DevOps pipeline, you'll use both — signed commits for traceability, encrypted secrets for security, and signed artifacts for supply chain integrity.
