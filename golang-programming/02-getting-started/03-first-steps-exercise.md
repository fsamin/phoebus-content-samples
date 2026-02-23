---
title: "First Steps with Go"
type: terminal-exercise
estimated_duration: "10m"
---

# First Steps with Go

Practice creating a Go project and running your first program.

## Step 1: Initialize a Go module

Create a new directory and initialize a Go module.

```console
$ ▌
```

- [x] `mkdir myapp && cd myapp && go mod init myapp` — Creates a directory and initializes a Go module.
- [ ] `go init myapp` — The command is `go mod init`, not `go init`.
- [ ] `go new myapp` — There is no `go new` command.

```output
go: creating new go.mod: module myapp
```

## Step 2: Run a Go program

Create and run a Hello World program.

```console
$ ▌
```

- [ ] `go compile main.go` — There is no `go compile` command.
- [x] `echo 'package main; import "fmt"; func main() { fmt.Println("Hello, Go!") }' > main.go && go run main.go` — Creates and runs a minimal Go program.
- [ ] `go exec main.go` — There is no `go exec` command.

```output
Hello, Go!
```

## Step 3: Build a binary

Compile the program into a standalone binary and run it.

```console
$ ▌
```

- [x] `go build -o myapp . && ./myapp` — Builds a binary named `myapp` and executes it.
- [ ] `go build && ./main` — The default binary name is the module name, not `main`.
- [ ] `go compile -o myapp main.go` — There is no `go compile` command.

```output
Hello, Go!
```
