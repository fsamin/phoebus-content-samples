---
title: "Create a Feature Branch"
type: terminal-exercise
estimated_duration: "10m"
---

# Create a Feature Branch

You're working on a project and need to implement a new feature. Follow the standard branching workflow.

## Step 1: Make sure you're on the latest main branch

Before creating a feature branch, ensure you have the latest code.

```console
$ ▌
```

- [x] `git checkout main && git pull origin main` — Switches to main and fetches the latest changes from the remote.
- [ ] `git checkout main` — Switches to main but doesn't fetch the latest remote changes.
- [ ] `git pull` — Pulls the current branch, but you might not be on main.

```output
Already on 'main'
Already up to date.
```

## Step 2: Create and switch to a new feature branch

Create a branch for adding user authentication.

```console
$ ▌
```

- [ ] `git branch feature/user-auth` — Creates the branch but doesn't switch to it.
- [x] `git switch -c feature/user-auth` — Creates and switches to the new branch in one command.
- [ ] `git checkout feature/user-auth` — Fails because the branch doesn't exist yet (missing -b flag).

```output
Switched to a new branch 'feature/user-auth'
```

## Step 3: Make a commit on your feature branch

You've added a login handler. Stage and commit it.

```console
$ ▌
```

- [ ] `git commit -m "add login"` — Nothing is staged, the commit would fail.
- [ ] `git add . && git commit` — Opens an editor but doesn't follow commit conventions.
- [x] `git add -A && git commit -m "feat(auth): add login handler"` — Stages all changes and commits with a conventional commit message.

```output
[feature/user-auth abc1234] feat(auth): add login handler
 2 files changed, 45 insertions(+)
 create mode 100644 internal/handler/login.go
```
