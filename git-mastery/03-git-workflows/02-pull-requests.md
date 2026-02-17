---
title: "Pull Requests & Code Review"
type: lesson
estimated_duration: "15m"
---

# Pull Requests & Code Review

## Why Code Review?

Code review is the #1 practice for maintaining code quality. It:

- **Catches bugs** before they reach production
- **Shares knowledge** across the team
- **Enforces standards** consistently
- **Documents decisions** in PR discussions

## Anatomy of a Good Pull Request

### Title

Follow the same Conventional Commits format:

```
feat(auth): add OAuth2 login with GitHub
fix(api): handle nil pointer in user handler
refactor(db): migrate from raw SQL to sqlx
```

### Description

```markdown
## What

Add OAuth2 authentication with GitHub as an identity provider.

## Why

Users requested GitHub login to avoid managing separate credentials.
Resolves #42.

## How

- Added `golang.org/x/oauth2` dependency
- Created `/auth/github/callback` endpoint
- Store GitHub user info in `users` table with `provider='github'`

## Testing

- [ ] Login with GitHub account
- [ ] Verify user is created in database
- [ ] Verify JWT token is issued
- [ ] Logout clears the session

## Screenshots

[Login page with GitHub button]
```

### Size

| PR Size | Lines Changed | Review Time | Quality |
|---------|--------------|-------------|---------|
| Small | < 200 | Quick, thorough | High |
| Medium | 200-500 | Reasonable | Good |
| Large | 500-1000 | Slow, superficial | Low |
| Huge | > 1000 | Rubber-stamped | Very Low |

**Rule of thumb:** If a PR takes more than 30 minutes to review, it's too big. Split it.

## Reviewing Code

### What to look for

1. **Correctness** — Does it do what it claims?
2. **Edge cases** — What happens with nil, empty, negative values?
3. **Security** — SQL injection, XSS, hardcoded secrets?
4. **Performance** — N+1 queries, unnecessary allocations?
5. **Readability** — Can a new team member understand it?
6. **Tests** — Are the changes tested? Are edge cases covered?

### How to comment

**Good:**
```
The SQL query builds the WHERE clause with string concatenation.
This is vulnerable to SQL injection. Use parameterized queries instead:
`db.Query("SELECT * FROM users WHERE id = $1", userID)`
```

**Bad:**
```
This is wrong. Fix it.
```

### Review etiquette

- **Be kind** — critique the code, not the person
- **Be specific** — suggest a concrete fix, not just "this is bad"
- **Be timely** — review within 24 hours
- **Acknowledge good work** — "Nice refactoring!" matters

## Summary

Pull requests are the collaboration hub of modern development. Write clear descriptions, keep PRs small, review thoughtfully, and be kind. The investment in review pays back in production stability.
