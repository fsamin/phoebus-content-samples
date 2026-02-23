---
title: "Secure Development Quiz"
type: quiz
estimated_duration: "10m"
---

# Secure Development Quiz

## [multiple-choice] Which of the following prevents SQL injection?

- [ ] Escaping quotes with `strings.ReplaceAll(input, "'", "''")`
- [ ] Using an ORM only
- [x] Using parameterized queries with `$1`, `$2` placeholders
- [ ] Validating that the input contains no spaces

> **Explanation:** Parameterized queries separate SQL code from data. The database driver handles escaping. Manual escaping is error-prone, and ORMs ultimately use parameterized queries under the hood — but you should understand *why* they work.

## [multiple-choice] What should you NEVER do with passwords?

- [ ] Hash them with bcrypt
- [x] Store them in plain text or hash them with MD5/SHA
- [ ] Use a cost factor of 12 or higher
- [ ] Salt them before hashing

> **Explanation:** MD5 and SHA are not designed for password hashing — they're too fast, making brute-force attacks feasible. Always use a purpose-built password hashing algorithm: bcrypt, scrypt, or argon2id.

## [short-answer] What Go tool checks your dependencies against the Go vulnerability database?

    govulncheck

> **Explanation:** `govulncheck` is maintained by the Go security team. It uses call graph analysis to determine if your code actually calls vulnerable functions, significantly reducing false positives compared to other dependency scanners.

## [multiple-choice] How should secrets (API keys, database passwords) be provided to a Go application?

- [ ] Hardcoded in a `config.go` file
- [ ] Committed in a `config.yaml` in the repository
- [x] Via environment variables or a secret manager
- [ ] In comments in the source code

> **Explanation:** Secrets must never be in source code or committed files. Use environment variables for simple setups, or a secret manager (HashiCorp Vault, AWS Secrets Manager, Kubernetes Secrets) for production. Always add `.env` files to `.gitignore`.

## [multiple-choice] What does `gosec` rule G201 detect?

- [ ] Use of weak TLS versions
- [ ] Missing error checks
- [x] SQL query construction using string concatenation
- [ ] Hardcoded IP addresses

> **Explanation:** G201 specifically flags SQL queries built with string concatenation (`"SELECT * FROM users WHERE id = " + id`), which is vulnerable to SQL injection. The fix is to use parameterized queries.

## [multiple-choice] What is the correct way to prevent command injection when running external commands?

- [ ] `exec.Command("sh", "-c", "git clone " + url)` with URL validation
- [ ] Escaping special characters in the URL
- [x] `exec.Command("git", "clone", url)` — passing arguments separately
- [ ] Using `os.System()` instead of `exec.Command`

> **Explanation:** When you pass arguments as separate strings to `exec.Command`, they are never interpreted by a shell. The command receives each argument literally, preventing injection. Never use `sh -c` with user input.
