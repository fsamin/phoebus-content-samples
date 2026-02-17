---
title: "Git Basics Quiz"
type: quiz
estimated_duration: "10m"
---

# Git Basics Quiz

## [multiple-choice] What are the three states of a file in Git?

- [ ] Created, Modified, Deleted
- [x] Working Directory, Staging Area, Repository
- [ ] Local, Remote, Cloud
- [ ] Draft, Review, Published

> **Explanation:** Files move from the Working Directory (edits) to the Staging Area (`git add`) to the Repository (`git commit`). Understanding this flow is fundamental to Git.

## [multiple-choice] What is the correct Conventional Commit format?

- [ ] `[FEAT] Add login page`
- [ ] `FEATURE: add login page`
- [x] `feat(auth): add login page`
- [ ] `Added: login page feature`

> **Explanation:** Conventional Commits use the format `type(scope): description`. The type is lowercase, the scope is optional but recommended, and the description starts with a lowercase verb.

## [short-answer] What command creates a new branch AND switches to it in one step?

    git switch -c branch-name

> **Explanation:** `git switch -c` (create) creates and switches in one command. The older syntax `git checkout -b` also works but `switch` is the modern alternative introduced in Git 2.23.

## [multiple-choice] What does `git push -u origin feature/login` do that `git push origin feature/login` doesn't?

- [x] Sets the upstream tracking branch so future `git push` and `git pull` work without specifying the remote
- [ ] Forces the push even if there are conflicts
- [ ] Creates the branch on the remote if it doesn't exist
- [ ] Updates all branches at once

> **Explanation:** The `-u` (or `--set-upstream`) flag links your local branch to the remote branch. After setting upstream, you can just type `git push` or `git pull` without specifying `origin feature/login`.

## [multiple-choice] Which file tells Git which files to ignore?

- [ ] `.gitconfig`
- [ ] `.gitmodules`
- [x] `.gitignore`
- [ ] `.gitexclude`

> **Explanation:** `.gitignore` is placed at the repository root (or in subdirectories) and uses glob patterns to specify files Git should not track. Common entries include `node_modules/`, `.env`, and build outputs.
