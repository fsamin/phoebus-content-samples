---
title: "File Permissions"
type: lesson
estimated_duration: "20m"
---

# File Permissions

## The Permission Model

Every file and directory in Linux has three sets of permissions for three categories of users:

| Category | Symbol | Description |
|----------|--------|-------------|
| **Owner** | `u` | The user who owns the file |
| **Group** | `g` | Users in the file's group |
| **Others** | `o` | Everyone else |

Each category can have three permissions:

| Permission | Symbol | File | Directory |
|------------|--------|------|-----------|
| **Read** | `r` (4) | View contents | List contents |
| **Write** | `w` (2) | Modify contents | Create/delete files |
| **Execute** | `x` (1) | Run as program | Enter directory |

## Reading Permissions

```bash
$ ls -la
drwxr-xr-x  2 user group 4096 Jan 15 10:30 scripts/
-rw-r--r--  1 user group  256 Jan 15 10:30 config.yaml
-rwxr-x---  1 user group 8192 Jan 15 10:30 deploy.sh
```

Breaking down `-rwxr-x---`:
- `-` — regular file (d = directory, l = symlink)
- `rwx` — owner can read, write, execute
- `r-x` — group can read and execute
- `---` — others have no access

## Octal Notation

Permissions can be expressed as octal numbers:

| Octal | Binary | Permissions |
|-------|--------|-------------|
| 7 | 111 | rwx |
| 6 | 110 | rw- |
| 5 | 101 | r-x |
| 4 | 100 | r-- |
| 0 | 000 | --- |

Common permission patterns:

| Octal | Meaning | Use Case |
|-------|---------|----------|
| `755` | rwxr-xr-x | Scripts, directories |
| `644` | rw-r--r-- | Configuration files |
| `600` | rw------- | Private keys, secrets |
| `700` | rwx------ | Private directories |

## Changing Permissions

```bash
# Symbolic notation
chmod u+x script.sh         # Add execute for owner
chmod g-w config.yaml        # Remove write for group
chmod o-rwx secret.key       # Remove all for others
chmod a+r README.md          # Add read for all

# Octal notation
chmod 755 deploy.sh          # rwxr-xr-x
chmod 600 id_rsa             # rw-------
chmod 644 nginx.conf         # rw-r--r--
```

## Changing Ownership

```bash
chown user:group file.txt        # Change owner and group
chown -R www-data:www-data /var/www  # Recursive ownership change
chgrp docker /var/run/docker.sock    # Change group only
```

## Special Permissions

| Permission | Octal | Effect |
|------------|-------|--------|
| **Setuid** | 4000 | Execute as file owner |
| **Setgid** | 2000 | Execute as file group / inherit group in directory |
| **Sticky** | 1000 | Only owner can delete files (used on `/tmp`) |

## DevOps Best Practices

1. **SSH keys**: Always `chmod 600 ~/.ssh/id_rsa` — SSH refuses keys with loose permissions
2. **Scripts**: Use `755` for shared scripts, `700` for sensitive ones
3. **Config files**: Use `644` for non-sensitive, `600` for secrets
4. **Never use `777`** — it's a security vulnerability

## Summary

Linux permissions are your first line of defense. Understanding and correctly setting permissions prevents unauthorized access and is critical for security compliance in production environments.
