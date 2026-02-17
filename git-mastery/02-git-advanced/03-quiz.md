---
title: "Git Advanced Quiz"
type: quiz
estimated_duration: "10m"
---

# Git Advanced Quiz

## [multiple-choice] What is the golden rule of rebase?

- [ ] Always rebase before merging
- [ ] Never rebase more than 5 commits
- [x] Never rebase commits that have been pushed to a shared branch
- [ ] Always use interactive rebase

> **Explanation:** Rebasing rewrites commit hashes. If others have based work on those commits, rebasing creates duplicate commits and history conflicts that are very difficult to resolve.

## [multiple-choice] What does `git rebase -i HEAD~3` do?

- [ ] Rebases the last 3 branches
- [ ] Merges 3 commits into one
- [x] Opens an interactive editor to rewrite the last 3 commits (pick, squash, reword, etc.)
- [ ] Deletes the last 3 commits

> **Explanation:** Interactive rebase (`-i`) opens your editor with the last 3 commits, letting you pick, squash, fixup, reword, edit, or drop each one. It's the standard way to clean up history before a PR.

## [short-answer] What command saves your uncommitted changes temporarily without creating a commit?

    git stash

> **Explanation:** `git stash` saves your working directory and staging area changes to a stack. Use `git stash pop` to restore them. Add `push -m "message"` for a descriptive name.

## [multiple-choice] What is the difference between `git reset --soft HEAD~1` and `git reset --hard HEAD~1`?

- [x] Soft keeps changes staged; hard discards all changes permanently
- [ ] Soft is faster; hard is slower
- [ ] Soft works on local branches; hard works on remote branches
- [ ] There is no difference

> **Explanation:** `--soft` moves HEAD back but leaves your changes staged (ready to re-commit). `--hard` moves HEAD back and wipes all changes from staging AND working directory — it's destructive.

## [multiple-choice] When would you use `git cherry-pick`?

- [ ] To merge two branches together
- [ ] To delete a specific commit
- [x] To apply a specific commit from another branch without merging the whole branch
- [ ] To create a new branch from a specific commit

> **Explanation:** Cherry-pick copies a specific commit to your current branch. The classic use case is backporting a bug fix from main to a release branch without bringing in all new features.
