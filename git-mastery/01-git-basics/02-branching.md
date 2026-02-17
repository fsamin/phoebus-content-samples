---
title: "Branching & Merging"
type: lesson
estimated_duration: "20m"
---

# Branching & Merging

## What are Branches?

Branches are lightweight pointers to commits. They let you work on features in isolation without affecting the main codebase.

```
main:    A---B---C
              \
feature:       D---E
```

## Branch Operations

```bash
# List branches
git branch          # Local branches
git branch -r       # Remote branches
git branch -a       # All branches

# Create and switch
git checkout -b feature/login
# or (modern Git)
git switch -c feature/login

# Switch to existing branch
git checkout main
git switch main

# Delete a branch
git branch -d feature/login      # Safe delete (only if merged)
git branch -D feature/login      # Force delete

# Rename current branch
git branch -m new-name
```

## Merging

### Fast-forward merge

When the target branch has no new commits:

```bash
git checkout main
git merge feature/login
```

```
Before:  main: A---B
                    \
         feat:       C---D

After:   main: A---B---C---D   (pointer moved forward)
```

### Three-way merge

When both branches have diverged:

```bash
git checkout main
git merge feature/login
```

```
Before:  main: A---B---E
                    \
         feat:       C---D

After:   main: A---B---E---F   (merge commit F)
                    \      /
         feat:       C---D
```

## Resolving Conflicts

When Git can't auto-merge, you get conflict markers:

```
<<<<<<< HEAD
const port = 8080;
=======
const port = 3000;
>>>>>>> feature/login
```

Resolution steps:

```bash
# 1. Edit the file to resolve conflicts
# 2. Stage the resolved file
git add server.js

# 3. Complete the merge
git commit

# Or abort the merge
git merge --abort
```

## Remote Branches

```bash
# Fetch remote changes
git fetch origin

# Pull = fetch + merge
git pull origin main

# Push a branch
git push origin feature/login

# Push and set upstream
git push -u origin feature/login

# Delete a remote branch
git push origin --delete feature/login
```

## Best Practices

1. **Keep branches short-lived** — merge within days, not weeks
2. **Branch from `main`** — always start from the latest stable code
3. **Use descriptive names** — `feature/user-auth`, `fix/memory-leak`, `chore/upgrade-deps`
4. **Delete merged branches** — keep the branch list clean
5. **Pull before push** — avoid unnecessary merge conflicts

## Summary

Branches are Git's killer feature. They're cheap to create, easy to merge, and essential for team collaboration. Master branching and you'll work confidently on any project.
