---
title: "Branching Strategies"
type: lesson
estimated_duration: "20m"
---

# Branching Strategies

## Why Branching Strategies Matter

Without a clear branching strategy, teams end up with:
- Long-lived branches that are painful to merge
- Unclear release processes
- Broken builds on the main branch
- "Works on my machine" problems

## GitFlow

Created by Vincent Driessen in 2010. Works well for projects with scheduled releases.

```
main ──────●───────────────────●──── (releases only)
            \                 /
develop ─────●───●───●───●───● ──── (integration branch)
              \     / \     /
feature/a ─────●───●   \   /
                        \ /
feature/b ────────────●──●
```

**Branches:**
| Branch | Purpose | Lifetime |
|--------|---------|----------|
| `main` | Production releases only | Permanent |
| `develop` | Integration branch | Permanent |
| `feature/*` | New features | Short-lived |
| `release/*` | Release preparation | Short-lived |
| `hotfix/*` | Production bug fixes | Short-lived |

**Pros:** Clear separation, good for versioned releases
**Cons:** Complex, slow for CI/CD, too many branches

## Trunk-Based Development

Teams commit directly to `main` (trunk) or use very short-lived feature branches (< 1 day).

```
main ──●──●──●──●──●──●──●──●──●── (continuous integration)
        \  /    \  /
feat     ●●      ●●     (merged within hours)
```

**Key practices:**
- Feature flags to hide incomplete features
- Small, frequent commits
- Automated testing on every commit
- No long-lived branches

**Pros:** Simple, fast feedback, ideal for CI/CD
**Cons:** Requires discipline, feature flags add complexity

## GitHub Flow

A simplified model used by GitHub itself. The most popular approach for web applications.

```
main ──●─────────●─────────●── (always deployable)
        \       / \       /
feature  ●──●──●   ●──●──●
         PR review   PR review
```

**Rules:**
1. `main` is always deployable
2. Create a feature branch from `main`
3. Open a Pull Request early
4. Review and discuss the code
5. Merge to `main` (squash or merge commit)
6. Deploy immediately

**Pros:** Simple, PR-driven, great for web apps
**Cons:** No explicit release process

## Comparison

| Aspect | GitFlow | Trunk-Based | GitHub Flow |
|--------|---------|-------------|-------------|
| Complexity | High | Low | Low |
| CI/CD fit | Poor | Excellent | Good |
| Release model | Scheduled | Continuous | Continuous |
| Team size | Any | Small-Medium | Any |
| Best for | Libraries, mobile apps | SaaS, microservices | Web apps, APIs |

## DevOps Recommendation

For most DevOps teams, **GitHub Flow** or **Trunk-Based Development** is the right choice:
- Fast feedback loops
- Continuous deployment compatibility
- Simple mental model
- Forces small, reviewable changes

Use **GitFlow** only if you have explicit versioned releases (mobile apps, libraries, on-premise software).

## Summary

Your branching strategy should match your deployment model. If you deploy continuously, use GitHub Flow or Trunk-Based. If you have release trains, consider GitFlow. The best strategy is the one your team actually follows consistently.
