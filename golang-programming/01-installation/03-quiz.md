---
title: "Go Toolchain Quiz"
type: quiz
estimated_duration: "5m"
---

# Go Toolchain Quiz

## [multiple-choice] Where should Go be installed when using the official tarball?

- [ ] `/opt/go`
- [x] `/usr/local/go`
- [ ] `~/go`
- [ ] `/usr/bin/go`

> **Explanation:** The official Go installation instructions recommend extracting the tarball to `/usr/local/go`. The `~/go` directory is `GOPATH`, which is your workspace — not the installation directory.

## [multiple-choice] What is the purpose of `GOPATH`?

- [ ] It points to the Go compiler installation
- [x] It defines the workspace for modules cache and installed binaries
- [ ] It sets the default build output directory
- [ ] It configures the Go proxy URL

> **Explanation:** `GOPATH` (default `~/go`) contains `bin/` (installed binaries), `pkg/mod/` (module cache). Since Go modules (Go 1.11+), you no longer need to keep your source code under `GOPATH`.

## [short-answer] What command shows all Go environment variables?

    go env

> **Explanation:** `go env` displays all Go environment variables including `GOROOT`, `GOPATH`, `GOOS`, `GOARCH`, `GOMODCACHE`, and more. You can also query a specific variable: `go env GOROOT`.

## [multiple-choice] Why is installing Go from the official tarball preferred over package managers?

- [ ] The tarball is smaller
- [ ] Package managers don't support Go
- [x] Package managers often ship outdated versions
- [ ] The tarball includes more tools

> **Explanation:** Linux distribution package managers often lag behind official releases by several versions. The Go team releases updates every ~6 months with important security fixes and performance improvements.
