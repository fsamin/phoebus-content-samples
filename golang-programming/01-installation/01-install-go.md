---
title: "Download and Install Go"
type: lesson
estimated_duration: "15m"
---

# Download and Install Go

## Why Install from the Tarball?

While package managers (`apt`, `dnf`, `snap`) can install Go, they often ship outdated versions. The official tarball from [go.dev](https://go.dev/dl/) guarantees you get the latest stable release.

## Step 1: Download the Tarball

Go to [https://go.dev/dl/](https://go.dev/dl/) and copy the link for the Linux AMD64 tarball, or use `curl`:

```bash
curl -LO https://go.dev/dl/go1.23.0.linux-amd64.tar.gz
```

> **Tip:** Always verify the checksum after downloading:
> ```bash
> sha256sum go1.23.0.linux-amd64.tar.gz
> ```

## Step 2: Remove Previous Installation and Extract

If you have a previous Go installation, remove it first:

```bash
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf go1.23.0.linux-amd64.tar.gz
```

The `-C /usr/local` flag tells `tar` to extract into `/usr/local`, creating `/usr/local/go/`.

## Step 3: Configure Environment Variables

Add Go to your `PATH` by editing `~/.bashrc` (or `~/.zshrc`):

```bash
export PATH=$PATH:/usr/local/go/bin
export GOPATH=$HOME/go
export PATH=$PATH:$GOPATH/bin
```

Then reload:

```bash
source ~/.bashrc
```

## Understanding the Go Directories

| Directory | Purpose |
|---|---|
| `/usr/local/go/` | Go installation (compiler, standard library, tools) |
| `$GOPATH/` (default `~/go/`) | Your workspace: downloaded modules, compiled binaries |
| `$GOPATH/bin/` | Installed Go binaries (tools like `golangci-lint`, `dlv`) |
| `$GOPATH/pkg/mod/` | Module cache (downloaded dependencies) |

## Step 4: Verify the Installation

```bash
go version
```

Expected output:

```
go version go1.23.0 linux/amd64
```

Check the environment:

```bash
go env GOROOT GOPATH
```

```
/usr/local/go
/home/devops/go
```
