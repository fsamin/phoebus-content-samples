---
title: "Generating Keys on Every OS"
type: lesson
estimated_duration: "20m"
---

# Generating Keys on Every OS

## Linux

OpenSSH is pre-installed on virtually all Linux distributions.

### Generate a Key

```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
```

You'll see:
```
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/user/.ssh/id_ed25519): ↵
Enter passphrase (empty for no passphrase): ********
Enter same passphrase again: ********
Your identification has been saved in /home/user/.ssh/id_ed25519
Your public key has been saved in /home/user/.ssh/id_ed25519.pub
```

### View Your Public Key

```bash
cat ~/.ssh/id_ed25519.pub
```

Output:
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIGhKxP3Y... your.email@example.com
```

### Set Correct Permissions

SSH is strict about file permissions. If they're wrong, SSH will refuse to use your keys.

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
chmod 600 ~/.ssh/authorized_keys  # if it exists
```

---

## macOS

macOS includes OpenSSH out of the box. The process is identical to Linux.

### Generate a Key

```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
```

Keys are stored in `~/.ssh/` just like on Linux.

### Integrate with macOS Keychain

macOS can store your SSH passphrase in the system Keychain so you don't have to retype it after reboot:

```bash
# Add key to the agent and store passphrase in Keychain
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

Add this to `~/.ssh/config` to make it persistent:

```
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

---

## Windows 10/11 (OpenSSH)

Since Windows 10 (October 2018 update), OpenSSH is built into Windows.

### Check OpenSSH is Installed

Open **PowerShell** and run:

```powershell
ssh -V
# Should output: OpenSSH_for_Windows_...
```

If not installed, enable it in **Settings → Apps → Optional Features → OpenSSH Client**.

### Generate a Key

Open **PowerShell** (not Command Prompt):

```powershell
ssh-keygen -t ed25519 -C "your.email@example.com"
```

Keys are stored in `C:\Users\YourName\.ssh\`.

### Start the SSH Agent

The SSH agent service is disabled by default on Windows. Enable and start it:

```powershell
# Run PowerShell as Administrator
Get-Service ssh-agent | Set-Service -StartupType Automatic
Start-Service ssh-agent

# Add your key
ssh-add $env:USERPROFILE\.ssh\id_ed25519
```

### View Your Public Key

```powershell
Get-Content $env:USERPROFILE\.ssh\id_ed25519.pub
```

---

## Windows (WSL — Windows Subsystem for Linux)

WSL (Windows Subsystem for Linux) lets you run a full Linux environment directly inside Windows. If you have WSL installed, you can use the native Linux SSH tools — which tend to handle permissions more reliably than Windows-native OpenSSH.

### Open a WSL Terminal

Launch your WSL distribution from the Start menu, Windows Terminal, or run `wsl` in PowerShell.

### Generate a Key

The process is identical to Linux:

```bash
ssh-keygen -t ed25519 -C "your.email@example.com"
```

### Key Location

Keys are stored in `~/.ssh/` inside the WSL filesystem (e.g., `/home/username/.ssh/`). This is a **separate** location from the Windows-native path `C:\Users\YourName\.ssh\` — the two are not shared automatically.

### Permissions

Unlike Windows-native OpenSSH, WSL respects Linux file permissions properly. Make sure your key permissions are correct:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

### Sharing Keys Between WSL and Windows

If you need the same key in both environments, you can copy between the two filesystems:

```bash
# Copy key from WSL to Windows
cp ~/.ssh/id_ed25519 /mnt/c/Users/YourName/.ssh/
cp ~/.ssh/id_ed25519.pub /mnt/c/Users/YourName/.ssh/

# Or copy from Windows to WSL (then fix permissions)
cp /mnt/c/Users/YourName/.ssh/id_ed25519 ~/.ssh/
chmod 600 ~/.ssh/id_ed25519
```

> 💡 **Tip:** If you mostly work inside WSL, generate and keep your keys in WSL. The Linux permission model is stricter and avoids common Windows permission pitfalls.

---

## Windows (with PuTTY/PuTTYgen)

If you use PuTTY (older method), key generation is different:

1. Open **PuTTYgen**
2. Select **EdDSA** (Ed25519) at the bottom
3. Click **Generate** and move your mouse to create randomness
4. Enter a **passphrase**
5. Click **Save private key** (saves as `.ppk` file — PuTTY format)
6. Copy the public key text from the top box

> ⚠️ PuTTY uses its own key format (`.ppk`). If you need an OpenSSH-format key, use **Conversions → Export OpenSSH key** in PuTTYgen. Modern Windows has OpenSSH built-in, so PuTTY is rarely needed anymore.

---

## Deploying Your Public Key to a Server

To authenticate with your key, the **public key** must be added to the server's `~/.ssh/authorized_keys` file.

### Method 1: ssh-copy-id (Linux/macOS)

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@server.example.com
```

This automatically appends your public key to the server's `authorized_keys`.

### Method 2: Manual Copy

```bash
# Copy your public key to clipboard
cat ~/.ssh/id_ed25519.pub

# On the server:
mkdir -p ~/.ssh
chmod 700 ~/.ssh
echo "ssh-ed25519 AAAAC3NzaC1..." >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

## Multiple Keys

You can have multiple key pairs for different purposes:

```bash
ssh-keygen -t ed25519 -C "work" -f ~/.ssh/id_work
ssh-keygen -t ed25519 -C "personal" -f ~/.ssh/id_personal
```

Configure which key to use for which server in `~/.ssh/config` (covered in the next module).
