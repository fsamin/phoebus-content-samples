---
title: "Linux Basics Quiz"
type: quiz
estimated_duration: "10m"
---

# Linux Basics Quiz

## [multiple-choice] What does the `/etc` directory contain?

- [ ] User home directories
- [x] System configuration files
- [ ] Temporary files
- [ ] Device files

> **Explanation:** `/etc` (et cetera) contains system-wide configuration files. Examples include `/etc/ssh/sshd_config`, `/etc/nginx/nginx.conf`, and `/etc/hosts`.

## [multiple-choice] What permission octal value represents `rwxr-xr-x`?

- [ ] 777
- [x] 755
- [ ] 644
- [ ] 700

> **Explanation:** `rwx` = 7 (4+2+1), `r-x` = 5 (4+0+1), `r-x` = 5. So the octal value is 755.

## [short-answer] Which command follows a log file in real-time, displaying new lines as they are written?

    tail -f

> **Explanation:** `tail -f` (follow) keeps the file open and displays new content as it's appended. This is essential for monitoring logs during deployments or troubleshooting.

## [multiple-choice] What does `chmod 600 id_rsa` do?

- [ ] Makes the file readable by everyone
- [ ] Makes the file executable by the owner
- [x] Gives read/write to the owner only, no access to others
- [ ] Gives read access to owner and group

> **Explanation:** 6 = rw- (read + write for owner), 0 = --- (no permissions for group), 0 = --- (no permissions for others). This is the required permission for SSH private keys.

## [multiple-choice] Which command pipeline counts the number of unique IP addresses in an access log?

- [ ] `cat access.log | grep IP`
- [ ] `wc -l access.log`
- [x] `awk '{print $1}' access.log | sort | uniq | wc -l`
- [ ] `find access.log -type ip`

> **Explanation:** This pipeline extracts the first field (IP address), sorts them, removes duplicates with `uniq`, then counts the remaining lines. This is a classic Unix pipeline pattern.

## [short-answer] What is the command to create a nested directory structure like `/opt/myapp/config/templates` in one command?

    mkdir -p /opt/myapp/config/templates

> **Explanation:** The `-p` flag (parents) creates all intermediate directories as needed. Without `-p`, mkdir would fail if `/opt/myapp` doesn't already exist.
