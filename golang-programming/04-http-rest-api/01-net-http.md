---
title: "The net/http Package"
type: lesson
estimated_duration: "25m"
---

# The net/http Package

Go's standard library includes a powerful HTTP server. No framework needed for most use cases.

## A Minimal HTTP Server

```go
package main

import (
    "fmt"
    "log"
    "net/http"
)

func main() {
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintln(w, "Hello, World!")
    })

    log.Println("Server listening on :8080")
    log.Fatal(http.ListenAndServe(":8080", nil))
}
```

## The `http.Handler` Interface

Everything in Go's HTTP ecosystem revolves around one interface:

```go
type Handler interface {
    ServeHTTP(ResponseWriter, *Request)
}
```

And its function adapter:

```go
type HandlerFunc func(ResponseWriter, *Request)
```

## Routing with `http.ServeMux` (Go 1.22+)

Since Go 1.22, the standard `ServeMux` supports **method matching** and **path parameters**:

```go
mux := http.NewServeMux()

// Method matching
mux.HandleFunc("GET /api/users", listUsers)
mux.HandleFunc("POST /api/users", createUser)
mux.HandleFunc("GET /api/users/{id}", getUser)
mux.HandleFunc("PUT /api/users/{id}", updateUser)
mux.HandleFunc("DELETE /api/users/{id}", deleteUser)

// Extract path parameter
func getUser(w http.ResponseWriter, r *http.Request) {
    id := r.PathValue("id")
    // ...
}
```

## Request and Response

### Reading the Request

```go
func createUser(w http.ResponseWriter, r *http.Request) {
    // Query parameters
    page := r.URL.Query().Get("page")

    // Headers
    auth := r.Header.Get("Authorization")

    // Body (JSON)
    var input CreateUserRequest
    if err := json.NewDecoder(r.Body).Decode(&input); err != nil {
        http.Error(w, "invalid JSON", http.StatusBadRequest)
        return
    }
}
```

### Writing the Response

```go
func getUser(w http.ResponseWriter, r *http.Request) {
    user := User{ID: 1, Name: "Gopher"}

    w.Header().Set("Content-Type", "application/json")
    w.WriteHeader(http.StatusOK)
    json.NewEncoder(w).Encode(user)
}
```

## Graceful Shutdown

A production server should handle shutdown signals:

```go
func main() {
    srv := &http.Server{
        Addr:         ":8080",
        Handler:      mux,
        ReadTimeout:  10 * time.Second,
        WriteTimeout: 30 * time.Second,
        IdleTimeout:  60 * time.Second,
    }

    // Start server in a goroutine
    go func() {
        if err := srv.ListenAndServe(); err != http.ErrServerClosed {
            log.Fatal(err)
        }
    }()

    // Wait for interrupt signal
    quit := make(chan os.Signal, 1)
    signal.Notify(quit, syscall.SIGINT, syscall.SIGTERM)
    <-quit

    log.Println("Shutting down...")
    ctx, cancel := context.WithTimeout(context.Background(), 30*time.Second)
    defer cancel()
    srv.Shutdown(ctx)
}
```
