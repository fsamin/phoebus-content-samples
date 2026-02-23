---
title: "Static Analysis and Dependency Scanning"
type: lesson
estimated_duration: "20m"
---

# Static Analysis and Dependency Scanning

## Why Automated Security Tools?

Manual code review catches design issues, but automated tools catch **patterns** at scale — across every file, every commit, every dependency.

## `go vet` — Built-in Static Analysis

`go vet` ships with Go and catches common mistakes:

```bash
go vet ./...
```

Examples it catches:
- `Printf` format string mismatches
- Unreachable code
- Suspicious uses of `sync/atomic`
- Copying mutex values

## `staticcheck` — Advanced Linter

The most comprehensive Go linter:

```bash
go install honnef.co/go/tools/cmd/staticcheck@latest
staticcheck ./...
```

Catches:
- Deprecated API usage
- Inefficient string operations
- Unused code
- Incorrect error handling patterns

## `gosec` — Security-Focused Linter

Specifically designed to find security issues:

```bash
go install github.com/securego/gosec/v2/cmd/gosec@latest
gosec ./...
```

### What `gosec` Detects

| Rule | Description |
|---|---|
| G101 | Hardcoded credentials |
| G201 | SQL query construction using string concatenation |
| G202 | SQL query using `Sprintf` |
| G301 | File permissions too open |
| G401 | Use of weak crypto (MD5, SHA1 for security) |
| G501 | Import of crypto/md5, crypto/sha1 |
| G601 | Implicit memory aliasing in for loops |

### Example Output

```
[/app/handler.go:42] - G201: SQL string concatenation (Confidence: HIGH)
   > query := "SELECT * FROM users WHERE id = " + id
```

## `govulncheck` — Dependency Vulnerability Scanner

Built by the Go team, checks your dependencies against the Go vulnerability database:

```bash
go install golang.org/x/vuln/cmd/govulncheck@latest
govulncheck ./...
```

### Key Feature: Call Graph Analysis

Unlike other scanners, `govulncheck` checks if your code **actually calls** the vulnerable function, reducing false positives:

```
Vulnerability #1: GO-2024-2687
  A remote attacker can cause a denial of service via HTTP/2
  Found in: golang.org/x/net@v0.17.0
  Fixed in: golang.org/x/net@v0.23.0
  Your code calls: golang.org/x/net/http2.(*Transport).RoundTrip
```

## CI/CD Integration

### GitHub Actions Example

```yaml
name: Security
on: [push, pull_request]

jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-go@v5
        with:
          go-version: '1.23'

      - name: Run go vet
        run: go vet ./...

      - name: Run staticcheck
        run: |
          go install honnef.co/go/tools/cmd/staticcheck@latest
          staticcheck ./...

      - name: Run gosec
        run: |
          go install github.com/securego/gosec/v2/cmd/gosec@latest
          gosec ./...

      - name: Run govulncheck
        run: |
          go install golang.org/x/vuln/cmd/govulncheck@latest
          govulncheck ./...
```

## Summary: Security Toolchain

| Tool | Purpose | When to Run |
|---|---|---|
| `go vet` | General correctness | Every build |
| `staticcheck` | Advanced linting | Every build |
| `gosec` | Security patterns | Every PR, CI |
| `govulncheck` | Dependency vulnerabilities | Every PR, weekly schedule |

:::tip
Integrate all four tools in your CI pipeline. They're fast (seconds), free, and catch real bugs before they reach production.
:::
