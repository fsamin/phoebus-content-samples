---
title: "Functions, Structs, and Interfaces"
type: lesson
estimated_duration: "30m"
---

# Functions, Structs, and Interfaces

## Functions

### Basic Functions

```go
func add(a, b int) int {
    return a + b
}
```

### Multiple Return Values

Go functions can return multiple values — idiomatically used for `(result, error)`:

```go
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("division by zero")
    }
    return a / b, nil
}

result, err := divide(10, 3)
if err != nil {
    log.Fatal(err)
}
```

### Named Return Values

```go
func parseConfig(path string) (host string, port int, err error) {
    // host, port, err are pre-declared
    // "naked return" returns them implicitly
    return
}
```

> **Best practice:** Use named returns only for short functions or documentation. Avoid naked returns in long functions.

### Variadic Functions

```go
func sum(numbers ...int) int {
    total := 0
    for _, n := range numbers {
        total += n
    }
    return total
}

sum(1, 2, 3)       // 6
sum(nums...)        // spread a slice
```

### Functions as Values

Functions are first-class citizens:

```go
handler := func(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintln(w, "Hello!")
}

http.HandleFunc("/", handler)
```

## Structs

Structs are Go's way to define custom types:

```go
type User struct {
    ID        int
    Username  string
    Email     string
    Active    bool
    CreatedAt time.Time
}
```

### Creating Structs

```go
// Named fields (preferred)
user := User{
    ID:       1,
    Username: "gopher",
    Email:    "gopher@go.dev",
    Active:   true,
}

// Zero value — all fields get their zero values
var emptyUser User  // ID: 0, Username: "", Active: false, etc.
```

### Methods

Methods are functions with a **receiver**:

```go
// Value receiver — works on a copy
func (u User) DisplayName() string {
    if u.Username != "" {
        return u.Username
    }
    return u.Email
}

// Pointer receiver — can modify the struct
func (u *User) Deactivate() {
    u.Active = false
}
```

**Rule of thumb:** Use a pointer receiver when you need to modify the struct, or when the struct is large.

### Struct Embedding (Composition)

Go favors **composition** over inheritance:

```go
type Admin struct {
    User          // embedded — Admin "inherits" User's fields and methods
    Permissions []string
}

admin := Admin{
    User:        User{Username: "admin"},
    Permissions: []string{"manage_users", "manage_repos"},
}

// Access embedded fields directly
fmt.Println(admin.Username)       // "admin"
fmt.Println(admin.DisplayName())  // "admin"
```

## Interfaces

Interfaces define behavior. In Go, interfaces are **implicit** — a type satisfies an interface simply by implementing its methods:

```go
type Writer interface {
    Write(p []byte) (n int, err error)
}
```

Any type with a `Write([]byte) (int, error)` method satisfies `Writer`. No `implements` keyword needed.

### Common Standard Library Interfaces

| Interface | Methods | Used By |
|---|---|---|
| `io.Reader` | `Read(p []byte) (n int, err error)` | Files, HTTP bodies, buffers |
| `io.Writer` | `Write(p []byte) (n int, err error)` | Files, HTTP responses, buffers |
| `fmt.Stringer` | `String() string` | Custom string representation |
| `error` | `Error() string` | All error types |
| `http.Handler` | `ServeHTTP(w, r)` | HTTP request handlers |

### Example: The `error` Interface

```go
type error interface {
    Error() string
}

// Custom error type
type NotFoundError struct {
    Resource string
    ID       string
}

func (e *NotFoundError) Error() string {
    return fmt.Sprintf("%s %s not found", e.Resource, e.ID)
}

// Usage
func findUser(id string) (*User, error) {
    // ...
    return nil, &NotFoundError{Resource: "user", ID: id}
}
```

### The Empty Interface and `any`

```go
// any is an alias for interface{} (Go 1.18+)
func printAnything(v any) {
    fmt.Printf("Type: %T, Value: %v\n", v, v)
}
```

### Type Assertions

```go
var w io.Writer = os.Stdout

// Type assertion
f, ok := w.(*os.File)
if ok {
    fmt.Println("Writing to file:", f.Name())
}

// Type switch
switch v := w.(type) {
case *os.File:
    fmt.Println("file:", v.Name())
case *bytes.Buffer:
    fmt.Println("buffer, length:", v.Len())
default:
    fmt.Println("unknown writer")
}
```
