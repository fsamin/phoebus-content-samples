---
title: "Install Go on Linux"
type: terminal-exercise
estimated_duration: "10m"
---

# Install Go on Linux

You have a fresh Linux server. Install Go from the official tarball and configure your environment.

## Step 1: Download the Go tarball

Download Go 1.23.0 for Linux AMD64.

```console
$ ▌
```

- [x] `curl -LO https://go.dev/dl/go1.23.0.linux-amd64.tar.gz` — Downloads the official tarball with redirect following.
- [ ] `apt install golang` — Uses the package manager which may have an outdated version.
- [ ] `wget go.dev/dl/go1.23.0` — Incomplete URL and missing file extension.

```output
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 68.7M  100 68.7M    0     0   112M      0 --:--:-- --:--:-- --:--:--  112M
```

## Step 2: Extract to /usr/local

Remove any existing Go installation and extract the tarball.

```console
$ ▌
```

- [ ] `tar -xzf go1.23.0.linux-amd64.tar.gz` — Extracts in the current directory instead of /usr/local.
- [x] `sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.23.0.linux-amd64.tar.gz` — Cleans up previous installation and extracts to the correct location.
- [ ] `sudo mv go1.23.0.linux-amd64.tar.gz /usr/local/go` — Moves the tarball instead of extracting it.

```output
```

## Step 3: Add Go to your PATH

Configure the environment variables permanently.

```console
$ ▌
```

- [ ] `PATH=$PATH:/usr/local/go/bin` — Only sets PATH for the current session.
- [x] `echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc && source ~/.bashrc` — Persists the PATH and reloads the shell configuration.
- [ ] `export GOROOT=/usr/local/go` — Sets GOROOT but doesn't add the binary to PATH.

```output
```

## Step 4: Verify the installation

Confirm Go is installed and accessible.

```console
$ ▌
```

- [x] `go version` — Shows the installed Go version.
- [ ] `which golang` — The binary is called `go`, not `golang`.
- [ ] `go --version` — Go uses `version` without dashes.

```output
go version go1.23.0 linux/amd64
```
