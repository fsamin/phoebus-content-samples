---
title: "Your First Go Program"
type: lesson
estimated_duration: "20m"
---

# Your First Go Program

## Initializing a Module

Every Go project starts with a **module**. A module is defined by a `go.mod` file at the root of your project:

```bash
mkdir hello && cd hello
go mod init github.com/yourname/hello
```

This creates `go.mod`:

```
module github.com/yourname/hello

go 1.23.0
```

## Hello, World!

Create `main.go`:

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

Run it:

```bash
go run main.go
```

Build a binary:

```bash
go build -o hello .
./hello
```

## Anatomy of a Go Program

| Element | Purpose |
|---|---|
| `package main` | Declares this file belongs to the `main` package (entry point) |
| `import "fmt"` | Imports the `fmt` package from the standard library |
| `func main()` | The entry point — execution starts here |

## Key Observations

- **No semicolons** — the compiler inserts them automatically
- **Unused imports are compile errors** — Go enforces clean code
- **`gofmt`** — Go has a single official code formatter. No debates about style:
  ```bash
  gofmt -w main.go
  ```

## Go Commands Cheat Sheet

| Command | Purpose |
|---|---|
| `go run` | Compile and run in one step |
| `go build` | Compile to a binary |
| `go mod init` | Initialize a new module |
| `go mod tidy` | Add missing / remove unused dependencies |
| `go fmt ./...` | Format all Go files |
| `go vet ./...` | Report suspicious constructs |
| `go test ./...` | Run all tests |
