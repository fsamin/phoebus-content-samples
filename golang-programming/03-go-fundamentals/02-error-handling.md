---
title: "Error Handling"
type: lesson
estimated_duration: "20m"
---

# Error Handling

## Philosophy

Go takes a deliberate stance: **errors are values**, not exceptions. Every function that can fail returns an `error` as its last return value. This makes the error path explicit and impossible to ignore.

```go
file, err := os.Open("config.yaml")
if err != nil {
    return fmt.Errorf("open config: %w", err)
}
defer file.Close()
```

## The `error` Interface

```go
type error interface {
    Error() string
}
```

The simplest way to create an error:

```go
errors.New("something went wrong")
fmt.Errorf("user %s not found", username)
```

## Wrapping Errors

Since Go 1.13, use `%w` to **wrap** errors — preserving the chain:

```go
func loadConfig(path string) (*Config, error) {
    data, err := os.ReadFile(path)
    if err != nil {
        return nil, fmt.Errorf("load config %s: %w", path, err)
    }
    // ...
}
```

The caller can unwrap:

```go
if errors.Is(err, os.ErrNotExist) {
    fmt.Println("config file does not exist")
}
```

## Sentinel Errors

Pre-defined errors used as constants:

```go
var (
    ErrNotFound     = errors.New("not found")
    ErrUnauthorized = errors.New("unauthorized")
    ErrForbidden    = errors.New("forbidden")
)

func GetUser(id string) (*User, error) {
    user, ok := users[id]
    if !ok {
        return nil, ErrNotFound
    }
    return user, nil
}

// Caller
user, err := GetUser("123")
if errors.Is(err, ErrNotFound) {
    // handle not found
}
```

## Custom Error Types

When you need structured error information:

```go
type ValidationError struct {
    Field   string
    Message string
}

func (e *ValidationError) Error() string {
    return fmt.Sprintf("validation error on %s: %s", e.Field, e.Message)
}

// Check with errors.As
var validErr *ValidationError
if errors.As(err, &validErr) {
    fmt.Printf("field %s: %s\n", validErr.Field, validErr.Message)
}
```

## The `defer` Statement

`defer` schedules a function call to run when the enclosing function returns. Essential for cleanup:

```go
func processFile(path string) error {
    f, err := os.Open(path)
    if err != nil {
        return err
    }
    defer f.Close() // guaranteed to run, even on error

    // process the file...
    return nil
}
```

Deferred calls execute in **LIFO** order (last in, first out).

## `panic` and `recover`

`panic` is for truly unrecoverable situations (programmer errors, not user errors):

```go
// panic — should be rare
func mustParseURL(raw string) *url.URL {
    u, err := url.Parse(raw)
    if err != nil {
        panic(fmt.Sprintf("invalid URL: %s", raw))
    }
    return u
}

// recover — catch panics (used in middleware)
func recoverMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        defer func() {
            if r := recover(); r != nil {
                http.Error(w, "Internal Server Error", 500)
                log.Printf("panic recovered: %v", r)
            }
        }()
        next.ServeHTTP(w, r)
    })
}
```

> **Rule:** Never use `panic` for expected error conditions. Return an `error` instead.
