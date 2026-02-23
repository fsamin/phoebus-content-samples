---
title: "Types, Variables, and Control Flow"
type: lesson
estimated_duration: "25m"
---

# Types, Variables, and Control Flow

## Variable Declarations

Go has several ways to declare variables:

```go
// Explicit type
var name string = "Phoebus"

// Type inference
var port = 8080

// Short declaration (most common, inside functions only)
host := "localhost"

// Constants
const maxRetries = 3
```

## Basic Types

| Type | Zero Value | Example |
|---|---|---|
| `bool` | `false` | `var active bool` |
| `string` | `""` | `name := "Go"` |
| `int`, `int64` | `0` | `count := 42` |
| `float64` | `0.0` | `pi := 3.14` |
| `byte` | `0` | Alias for `uint8` |
| `rune` | `0` | Alias for `int32` (Unicode code point) |

## Composite Types

### Slices (dynamic arrays)

```go
// Create a slice
names := []string{"Alice", "Bob", "Charlie"}

// Append
names = append(names, "Diana")

// Length and capacity
fmt.Println(len(names), cap(names))

// Iterate
for i, name := range names {
    fmt.Printf("%d: %s\n", i, name)
}
```

### Maps

```go
// Create a map
config := map[string]string{
    "host": "localhost",
    "port": "8080",
}

// Access
host := config["host"]

// Check existence
port, ok := config["port"]
if !ok {
    fmt.Println("port not configured")
}

// Delete
delete(config, "port")
```

## Control Flow

### If / Else

```go
if status == http.StatusOK {
    fmt.Println("success")
} else if status == http.StatusNotFound {
    fmt.Println("not found")
} else {
    fmt.Printf("unexpected status: %d\n", status)
}
```

Go supports **init statements** in `if`:

```go
if err := doSomething(); err != nil {
    log.Fatal(err)
}
// err is not accessible here
```

### For (the only loop)

```go
// Classic for
for i := 0; i < 10; i++ {
    fmt.Println(i)
}

// While-style
for count > 0 {
    count--
}

// Infinite loop
for {
    // break when done
}

// Range over slice
for index, value := range items {
    fmt.Printf("%d: %v\n", index, value)
}

// Range over map
for key, value := range config {
    fmt.Printf("%s = %s\n", key, value)
}
```

### Switch

```go
switch role {
case "admin":
    fmt.Println("full access")
case "instructor":
    fmt.Println("content + analytics")
default:
    fmt.Println("learner access")
}
```

Go's `switch` does **not** fall through by default (no `break` needed).

## Pointers

Go has pointers but no pointer arithmetic:

```go
name := "Phoebus"
ptr := &name       // ptr is *string, points to name
fmt.Println(*ptr)  // dereference: "Phoebus"

*ptr = "Phoenix"
fmt.Println(name)  // "Phoenix" — name was modified
```

When to use pointers:
- To **modify** a value in a function
- To avoid **copying** large structs
- When a value can be **nil** (absence of value)
