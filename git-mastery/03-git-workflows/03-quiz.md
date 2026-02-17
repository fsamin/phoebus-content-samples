---
title: "Git Workflows Quiz"
type: quiz
estimated_duration: "10m"
---

# Git Workflows Quiz

## [multiple-choice] Which branching strategy is best suited for continuous deployment of a web application?

- [ ] GitFlow
- [ ] Release branching
- [x] GitHub Flow or Trunk-Based Development
- [ ] No branching strategy needed

> **Explanation:** GitHub Flow and Trunk-Based Development are designed for continuous delivery. They keep the main branch always deployable and encourage small, frequent integrations. GitFlow adds overhead that slows down continuous deployment.

## [multiple-choice] What is the recommended maximum size for a pull request?

- [ ] As large as needed to complete the feature
- [x] Under 200 lines of changed code
- [ ] Exactly one file
- [ ] Under 1000 lines

> **Explanation:** Research shows that review quality drops significantly above 200-400 lines. Small PRs get reviewed faster, more thoroughly, and are less likely to introduce bugs. Break large features into multiple PRs.

## [short-answer] In GitFlow, what is the permanent branch used for integrating features before release?

    develop

> **Explanation:** GitFlow uses two permanent branches: `main` (production releases only) and `develop` (integration of features). Feature branches are merged into `develop`, then `develop` is merged into `main` via a release branch.

## [multiple-choice] When reviewing code, what should you prioritize?

- [ ] Code formatting and style
- [ ] Variable naming
- [x] Correctness, security, and edge cases
- [ ] Number of comments left

> **Explanation:** While style matters, automated linters handle it better. Human reviewers should focus on what tools can't catch: logic errors, security vulnerabilities, unhandled edge cases, and architectural concerns.

## [multiple-choice] In Trunk-Based Development, how are incomplete features handled?

- [ ] Long-lived feature branches
- [ ] Separate development environment
- [x] Feature flags that hide incomplete features in production
- [ ] They are not allowed

> **Explanation:** Feature flags (feature toggles) let you merge incomplete code to main without exposing it to users. This enables continuous integration while keeping the codebase deployable at all times.
