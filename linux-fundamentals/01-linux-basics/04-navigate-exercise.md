---
title: "Navigate the Filesystem"
type: terminal-exercise
estimated_duration: "10m"
---

# Navigate the Filesystem

You've just connected to a freshly provisioned Linux server. Your first task is to explore the system and find important configuration files.

## Step 1: Find your current location

Where are you in the filesystem? Print your current working directory.

```console
$ ▌
```

- [x] `pwd` — Prints the absolute path of the current working directory.
- [ ] `ls` — Lists directory contents but doesn't show your location.
- [ ] `whoami` — Shows your username, not your location.

```output
/home/devops
```

## Step 2: List all files including hidden ones

Your home directory might contain hidden configuration files (dotfiles). List everything.

```console
$ ▌
```

- [ ] `ls` — Only shows non-hidden files.
- [x] `ls -la` — Shows all files including hidden ones, with details.
- [ ] `ls -l` — Shows details but skips hidden files.

```output
total 24
drwxr-xr-x 3 devops devops 4096 Jan 15 10:30 .
drwxr-xr-x 4 root   root   4096 Jan 15 10:00 ..
-rw-r--r-- 1 devops devops  220 Jan 15 10:00 .bash_logout
-rw-r--r-- 1 devops devops 3771 Jan 15 10:00 .bashrc
drwx------ 2 devops devops 4096 Jan 15 10:30 .ssh
-rw-r--r-- 1 devops devops  807 Jan 15 10:00 .profile
```

## Step 3: Find all configuration files on the system

Locate all `.conf` files under `/etc` that contain the word "listen".

```console
$ ▌
```

- [ ] `find /etc -name "*.conf"` — Finds conf files but doesn't search contents.
- [ ] `ls /etc/*.conf` — Only lists files in /etc, not subdirectories.
- [x] `grep -rl "listen" /etc/*.conf 2>/dev/null` — Recursively searches for "listen" in .conf files.

```output
/etc/ssh/sshd_config.conf
```
