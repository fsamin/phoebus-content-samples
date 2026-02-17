---
title: "Essential Commands"
type: lesson
estimated_duration: "25m"
---

# Essential Linux Commands

## File Manipulation

### Creating and Viewing Files

```bash
touch file.txt          # Create empty file or update timestamp
echo "hello" > file.txt # Write text to file (overwrites)
echo "world" >> file.txt # Append text to file
cat file.txt            # Display file contents
less file.txt           # Page through file contents
head -n 5 file.txt      # Show first 5 lines
tail -n 5 file.txt      # Show last 5 lines
tail -f /var/log/syslog # Follow log file in real-time
```

### Copying, Moving, and Deleting

```bash
cp file.txt backup.txt        # Copy file
cp -r dir1/ dir2/             # Copy directory recursively
mv file.txt new_name.txt      # Rename / move file
rm file.txt                   # Delete file
rm -rf directory/             # Delete directory recursively (careful!)
mkdir -p path/to/dir          # Create nested directories
```

## Searching and Filtering

### Find Files

```bash
find / -name "*.conf"              # Find files by name
find /var/log -mtime -1            # Files modified in last 24h
find . -type f -size +100M         # Files larger than 100MB
find . -name "*.log" -delete       # Find and delete
```

### Search File Contents

```bash
grep "error" /var/log/syslog       # Search for pattern
grep -r "TODO" ./src/              # Recursive search
grep -i "warning" app.log          # Case-insensitive
grep -c "404" access.log           # Count matches
grep -n "panic" main.go            # Show line numbers
```

## Text Processing

```bash
# Sort and deduplicate
sort file.txt | uniq

# Count lines, words, characters
wc -l file.txt

# Extract columns
cut -d: -f1 /etc/passwd

# Stream editor — replace text
sed 's/old/new/g' file.txt

# Pattern scanning
awk '{print $1, $3}' access.log

# Combine with pipes
cat access.log | grep "POST" | awk '{print $7}' | sort | uniq -c | sort -rn
```

## Process Management

```bash
ps aux                    # List all processes
ps aux | grep nginx       # Find specific process
top                       # Interactive process viewer
htop                      # Better interactive viewer
kill <PID>                # Send SIGTERM to process
kill -9 <PID>             # Force kill (SIGKILL)
systemctl status nginx    # Check service status
systemctl restart nginx   # Restart a service
journalctl -u nginx -f    # Follow service logs
```

## Disk and System Info

```bash
df -h                     # Disk usage by filesystem
du -sh /var/log           # Directory size
free -h                   # Memory usage
uname -a                  # System information
uptime                    # System uptime and load
```

## Summary

These commands form your daily toolkit. The real power comes from combining them with pipes (`|`) and redirections (`>`, `>>`, `<`). Practice building command pipelines — it's the essence of the Unix philosophy.
