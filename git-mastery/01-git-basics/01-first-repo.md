---
title: "Your First Repository"
type: lesson
estimated_duration: "20m"
---

# Your First Repository

## What is Git?

Git is a **distributed version control system** created by Linus Torvalds in 2005. Every Git clone is a full repository with complete history — no single point of failure.

## The Three States

Git files exist in three states:

```
Working Directory → Staging Area → Repository
     (edit)          (git add)      (git commit)
```

1. **Working Directory** — your files on disk
2. **Staging Area (Index)** — snapshot of what will go into the next commit
3. **Repository (.git)** — committed snapshots stored permanently

## Creating a Repository

### From scratch

```bash
mkdir my-project && cd my-project
git init
```

### Clone an existing repo

```bash
git clone https://github.com/company/project.git
git clone git@github.com:company/project.git  # SSH (preferred)
```

## Making Commits

```bash
# Check status
git status

# Stage specific files
git add main.go
git add config/

# Stage everything
git add -A

# Commit with message
git commit -m "feat: initial project setup"

# Stage and commit tracked files in one step
git commit -am "fix: correct database connection string"
```

## Writing Good Commit Messages

Follow the **Conventional Commits** format:

```
type(scope): description

[optional body]

[optional footer(s)]
```

Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `ci`

**Good examples:**
```
feat(auth): add OAuth2 login with GitHub
fix(api): handle nil pointer in user handler
docs(readme): add deployment instructions
ci(github): add Go lint workflow
```

**Bad examples:**
```
fixed stuff
update
WIP
asdfgh
```

## Viewing History

```bash
# Compact log
git log --oneline

# Graph view
git log --oneline --graph --all

# Show changes in each commit
git log -p

# Show specific file history
git log --follow main.go
```

## The .gitignore File

Tell Git which files to ignore:

```gitignore
# Build outputs
/bin/
/dist/
*.exe

# Dependencies
/node_modules/
/vendor/

# IDE files
.idea/
.vscode/
*.swp

# Environment files
.env
*.local

# OS files
.DS_Store
Thumbs.db
```

## Summary

`git init`, `git add`, `git commit` — these three commands are the foundation. Master them before moving to branching, and always write meaningful commit messages.
