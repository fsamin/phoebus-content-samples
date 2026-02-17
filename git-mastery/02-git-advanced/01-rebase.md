---
title: "Rebase & Interactive Rebase"
type: lesson
estimated_duration: "20m"
---

# Rebase & Interactive Rebase

## Rebase vs Merge

Both integrate changes from one branch into another, but they create different histories.

### Merge (preserves history)

```
main:    A---B---C-------F (merge commit)
              \          /
feature:       D---E----
```

### Rebase (linear history)

```
Before:  main: A---B---C
                    \
         feat:       D---E

After:   main: A---B---C
                        \
         feat:           D'---E'  (replayed on top of C)
```

## Basic Rebase

```bash
# On your feature branch
git checkout feature/login
git rebase main

# Then fast-forward merge on main
git checkout main
git merge feature/login   # Fast-forward, no merge commit!
```

## Interactive Rebase

Clean up your commit history before merging:

```bash
git rebase -i HEAD~4    # Rebase last 4 commits
git rebase -i main      # Rebase all commits since diverging from main
```

The editor opens with your commits:

```
pick abc1234 feat(auth): add login form
pick def5678 fix typo in login form
pick ghi9012 feat(auth): add password validation
pick jkl3456 fix another typo
```

### Commands

| Command | Effect |
|---------|--------|
| `pick` | Keep the commit as-is |
| `reword` | Keep the commit but edit the message |
| `squash` | Merge into previous commit (combine messages) |
| `fixup` | Merge into previous commit (discard message) |
| `drop` | Delete the commit |
| `edit` | Pause to amend the commit |

### Example: Clean up before PR

```
pick abc1234 feat(auth): add login form
fixup def5678 fix typo in login form
pick ghi9012 feat(auth): add password validation
fixup jkl3456 fix another typo
```

Result: 2 clean commits instead of 4 messy ones.

## The Golden Rule of Rebase

> **Never rebase commits that have been pushed to a shared branch.**

Rebasing rewrites commit hashes. If others have based work on those commits, you'll create a nightmare of duplicate commits and conflicts.

**Safe to rebase:**
- Your local feature branch before pushing
- Your feature branch after `git push --force-with-lease` (if you're the only one working on it)

**Never rebase:**
- `main`, `develop`, or any shared branch
- Commits that others may have pulled

## Handling Rebase Conflicts

```bash
# Start rebase
git rebase main

# If conflicts occur:
# 1. Edit the conflicting files
# 2. Stage the resolved files
git add resolved-file.go

# 3. Continue the rebase
git rebase --continue

# Or abort the entire rebase
git rebase --abort
```

## Summary

Rebase creates clean, linear history — ideal for feature branches before merge. Interactive rebase lets you craft a professional commit history. But always respect the golden rule: never rebase shared commits.
