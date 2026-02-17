---
title: "The Linux Filesystem"
type: lesson
estimated_duration: "20m"
---

# The Linux Filesystem

## Everything is a File

In Linux, everything is represented as a file — regular files, directories, devices, and even processes. This philosophy simplifies how the system works and how you interact with it.

## The Filesystem Hierarchy

The Linux filesystem follows the **Filesystem Hierarchy Standard (FHS)**:

| Path | Purpose |
|------|---------|
| `/` | Root of the filesystem |
| `/home` | User home directories |
| `/etc` | System configuration files |
| `/var` | Variable data (logs, databases, mail) |
| `/tmp` | Temporary files (cleared on reboot) |
| `/usr` | User programs and data |
| `/bin` | Essential command binaries |
| `/sbin` | System administration binaries |
| `/opt` | Optional/third-party software |
| `/proc` | Virtual filesystem for process info |
| `/dev` | Device files |

## Absolute vs Relative Paths

- **Absolute path**: starts from root `/`, e.g., `/home/user/documents/report.txt`
- **Relative path**: relative to current directory, e.g., `../documents/report.txt`

## Key Directories for DevOps

As a DevOps engineer, you'll frequently work with:

- `/etc/` — configuration files (nginx, ssh, systemd)
- `/var/log/` — application and system logs
- `/tmp/` — temporary build artifacts
- `/opt/` — where you install tools like Prometheus, Grafana
- `/usr/local/bin/` — locally installed binaries

## Navigation Commands

```bash
pwd          # Print working directory
ls           # List directory contents
ls -la       # List all files with details
cd /etc      # Change to /etc directory
cd ~         # Change to home directory
cd -         # Change to previous directory
tree         # Display directory tree structure
```

## Summary

Understanding the filesystem hierarchy is fundamental. Every configuration change, log investigation, and deployment you'll do as a DevOps engineer starts with knowing where things live on the system.
