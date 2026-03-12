---
title: "The Terminal: your essential tool"
type: lesson
estimated_duration: "10m"
---

# The Terminal: your essential tool

## What is a terminal?

Imagine your computer speaks two languages:

- **The visual language**: you click on icons, drag and drop files, and use menus. That's what you do every day.
- **The text language**: you type written commands, and the computer responds… in text as well.

The **terminal** (also called **shell** or **console**) is simply the window that lets you talk to your computer using text. Don't worry, it's not just for experts! It's actually often faster and more precise than clicking around.

> 💡 **Analogy**: If your computer were a restaurant, the graphical interface would be the menu with pictures of the dishes. The terminal is when you speak directly to the chef: "I'd like pasta, al dente, with parmesan." More direct, more precise.

---

## How to open a terminal

| System | How to do it |
|---------|--------------|
| **Linux** | Press `Ctrl + Alt + T`, or search for "Terminal" in your applications |
| **macOS** | Press `Cmd + Space` (Spotlight), then type "Terminal" and press Enter |
| **Windows** | Open the Start menu and search for "PowerShell". Even better: install **WSL** (Windows Subsystem for Linux) to get a real Linux terminal |

Once opened, you'll see something like this:

```bash
user@my-computer:~$
```

This small text is called the **prompt**. It tells you "I'm ready, type something!". The `$` symbol at the end indicates that it's your turn to speak.

---

## Essential commands

You don't need to know hundreds of them. With these five commands, you can already do a lot.

### `pwd` — Where am I?

The `pwd` command (_Print Working Directory_) tells you which directory you are currently in.

```bash
$ pwd
/home/user
```

> 🗺️ It's like asking "where am I?" when you're lost in a building.

### `ls` — What's in here?

The `ls` command (_List_) displays the contents of the current directory.

```bash
$ ls
Desktop  Documents  Pictures  Music  Downloads
```

You can add options to get more details:

```bash
$ ls -l
total 20
drwxr-xr-x 2 user user 4096 Jan 15 10:00 Desktop
drwxr-xr-x 2 user user 4096 Jan 15 10:00 Documents
drwxr-xr-x 2 user user 4096 Jan 15 10:00 Pictures
```

> Don't worry about all those letters and numbers on the left, we'll come back to them below!

### `cd` — Moving around

The `cd` command (_Change Directory_) lets you move between directories.

```bash
$ cd Documents
$ pwd
/home/user/Documents
```

To go back to the previous directory (the parent directory):

```bash
$ cd ..
$ pwd
/home/user
```

> 🚶 Think of `cd` like walking down a hallway: `cd Documents` = "I'm entering the Documents room". `cd ..` = "I'm going back to the hallway".

### `cat` — Reading a file

The `cat` command displays the contents of a file directly in the terminal.

```bash
$ cat notes.txt
This is the content of my file.
It can contain multiple lines.
```

> It's like opening a text file, but without leaving the terminal.

### `mkdir` — Creating a directory

The `mkdir` command (_Make Directory_) creates a new directory.

```bash
$ mkdir my-project
$ ls
Desktop  Documents  Pictures  my-project  Music
```

The `my-project` directory is now created, ready to be used.

---

## Paths: knowing where you're going

When you type a command, the computer needs to know **where** to look. This is the concept of a **path**.

### Absolute path

An absolute path always starts from the **root** of the system (the very first level, denoted `/`). It describes the complete route.

```bash
/home/user/Documents/my-file.txt
```

> 📬 It's like a full mailing address: "123 Main Street, New York, USA".

### Relative path

A relative path starts from **where you currently are**. We often use `./` to mean "here".

```bash
./Documents/my-file.txt
```

> It's like saying "the door on the left" when you're already in the right hallway.

### The home directory: `~`

The `~` (tilde) symbol is a shortcut for your **home directory**. For example:

```bash
$ cd ~
$ pwd
/home/user
```

These two commands do exactly the same thing:

```bash
cd /home/user/Documents
cd ~/Documents
```

| Type | Example | Starts with |
|------|---------|-------------|
| Absolute | `/home/user/Documents` | `/` (the root) |
| Relative | `./Documents` or `Documents` | `.` or directly the name |
| Home | `~/Documents` | `~` (your home) |

---

## File permissions

Every file and directory on your system has **permissions**: rules that say who is allowed to do what. It's a bit like apartment keys: not everyone can get in everywhere.

### The two questions of permissions

Permissions answer **two questions**:

1. **Who?** — who does the rule apply to?
2. **What?** — what action is allowed?

#### Who: the three user levels

| Symbol | Who | Explanation |
|---------|-----|------------|
| **u** | _Owner_ | The person who created the file |
| **g** | _Group_ | A set of users defined by the administrator |
| **o** | _Others_ | Everyone else |

#### What: the three types of actions

| Symbol | Action | On a file | On a directory |
|---------|--------|---------------|----------------|
| **r** | _Read_ | View the contents | List the files |
| **w** | _Write_ | Modify the contents | Add or remove files |
| **x** | _Execute_ | Run as a program | Enter the directory |

### Reading permissions

When you run `ls -l`, the permissions appear on the left:

```bash
$ ls -l my-key.txt
-rw------- 1 user user 1679 Jan 15 10:00 my-key.txt
```

Let's break down `-rw-------`:

```
-  rw-  ---  ---
│   │    │    │
│   │    │    └── Others: no permissions
│   │    └─────── Group: no permissions
│   └──────────── Owner: read + write
└──────────────── Type (- = file, d = directory)
```

### Numeric notation with `chmod`

The `chmod` command lets you **change permissions**. The numeric notation uses digits from 0 to 7:

| Digit | Permissions | Calculation |
|---------|------------|--------|
| **0** | None | 0 |
| **1** | Execute only | x = 1 |
| **2** | Write only | w = 2 |
| **4** | Read only | r = 4 |
| **5** | Read + execute | r(4) + x(1) = 5 |
| **6** | Read + write | r(4) + w(2) = 6 |
| **7** | All (read + write + execute) | r(4) + w(2) + x(1) = 7 |

Each digit corresponds to a user level, in order: **owner**, **group**, **others**.

#### Example: `chmod 600`

```bash
$ chmod 600 my-key.txt
```

This gives:
- **6** → Owner: read + write (r + w)
- **0** → Group: no permissions
- **0** → Others: no permissions

Result: only the owner can read and modify the file. No one else can touch it.

### Why SSH cares about permissions

When you use SSH (that's the main topic of this course!), you'll be handling **connection keys**. These keys are like your house keys: if anyone can copy them, they become useless.

SSH is very strict about permissions. If your key files are too "open" (readable by other people), SSH will refuse to use them with a message like:

```
WARNING: UNPROTECTED PRIVATE KEY FILE!
Permissions 0644 for 'my-key' are too open.
```

The fix is simple:

```bash
$ chmod 600 my-key
```

> 🔒 Remember: **`chmod 600`** = "this file is mine and mine alone". This is the permission you'll use most often with SSH.

---

## Summary

| Command | What it does | Example |
|----------|----------------|---------|
| `pwd` | Shows the current directory | `pwd` → `/home/user` |
| `ls` | Lists the contents of a directory | `ls -l` → file details |
| `cd` | Changes directory | `cd ~/Documents` |
| `cat` | Displays the contents of a file | `cat notes.txt` |
| `mkdir` | Creates a directory | `mkdir my-project` |
| `chmod` | Changes permissions | `chmod 600 my-key` |

You now have the basics to navigate and work in a terminal. No need to memorize everything: with practice, these commands will become second nature. 💪
