---
title: "Cherry-pick, Stash & Reset"
type: lesson
estimated_duration: "15m"
---

# Cherry-pick, Stash & Reset

## Cherry-pick

Apply a specific commit from one branch to another without merging the entire branch.

```bash
# Apply a single commit
git cherry-pick abc1234

# Apply without committing (stage only)
git cherry-pick --no-commit abc1234

# Cherry-pick a range
git cherry-pick abc1234..def5678
```

**Use cases:**
- Backport a bug fix to a release branch
- Pull a specific feature commit into a hotfix branch
- Recover a commit from a deleted branch

### Example: Hotfix backport

```bash
# The fix was made on main
git log --oneline main
# abc1234 fix(api): patch SQL injection vulnerability

# Backport to release/v1.2
git checkout release/v1.2
git cherry-pick abc1234
```

## Stash

Temporarily save uncommitted changes to switch context.

```bash
# Stash current changes
git stash

# Stash with a descriptive message
git stash push -m "WIP: login form validation"

# List all stashes
git stash list

# Apply the most recent stash (keep in stash list)
git stash apply

# Apply and remove from stash list
git stash pop

# Apply a specific stash
git stash apply stash@{2}

# Drop a specific stash
git stash drop stash@{0}

# Clear all stashes
git stash clear
```

**Common workflow:**

```bash
# Working on feature, urgent bug comes in
git stash push -m "WIP: user dashboard"
git checkout main
git checkout -b hotfix/critical-bug
# ... fix the bug, commit, merge ...
git checkout feature/dashboard
git stash pop
# Back to where you were!
```

## Reset

Move the branch pointer and optionally modify staging/working directory.

### Three modes

```bash
# Soft: move HEAD, keep staging and working directory
git reset --soft HEAD~1
# Use case: "undo" a commit but keep changes staged

# Mixed (default): move HEAD, reset staging, keep working directory
git reset HEAD~1
# Use case: "undo" a commit and unstage changes

# Hard: move HEAD, reset staging AND working directory
git reset --hard HEAD~1
# Use case: completely discard the last commit and all changes
```

> ⚠️ `git reset --hard` is destructive. Use `git reflog` to recover if needed.

### Reflog — your safety net

```bash
# Show all HEAD movements
git reflog

# Recover a "lost" commit
git reset --hard abc1234    # Move back to the commit found in reflog
```

## Summary

- **Cherry-pick** — surgical commit transplantation
- **Stash** — context switching without committing
- **Reset** — rewriting local history (with caution)
- **Reflog** — your undo button for almost everything
