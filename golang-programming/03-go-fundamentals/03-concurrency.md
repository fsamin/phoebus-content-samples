---
title: "Concurrency Basics"
type: lesson
estimated_duration: "25m"
---

# Concurrency Basics

## Goroutines

A goroutine is a lightweight thread managed by the Go runtime. Start one with the `go` keyword:

```go
go func() {
    fmt.Println("running in background")
}()
```

Goroutines are extremely cheap — you can launch thousands:

```go
for i := 0; i < 1000; i++ {
    go processItem(i)
}
```

## Channels

Channels are Go's way to communicate between goroutines:

```go
// Create a channel
ch := make(chan string)

// Send (in a goroutine)
go func() {
    ch <- "hello"
}()

// Receive (blocks until a value is available)
msg := <-ch
fmt.Println(msg) // "hello"
```

### Buffered Channels

```go
ch := make(chan int, 10) // buffer of 10

// Sends don't block until the buffer is full
ch <- 1
ch <- 2
```

### Channel Patterns

**Fan-out / Fan-in:**

```go
func worker(id int, jobs <-chan int, results chan<- int) {
    for j := range jobs {
        results <- j * 2
    }
}

func main() {
    jobs := make(chan int, 100)
    results := make(chan int, 100)

    // Start 3 workers
    for w := 0; w < 3; w++ {
        go worker(w, jobs, results)
    }

    // Send jobs
    for j := 0; j < 9; j++ {
        jobs <- j
    }
    close(jobs)

    // Collect results
    for r := 0; r < 9; r++ {
        fmt.Println(<-results)
    }
}
```

## WaitGroup

Wait for multiple goroutines to finish:

```go
var wg sync.WaitGroup

for _, url := range urls {
    wg.Add(1)
    go func(u string) {
        defer wg.Done()
        resp, err := http.Get(u)
        if err != nil {
            log.Println(err)
            return
        }
        resp.Body.Close()
        fmt.Printf("%s: %d\n", u, resp.StatusCode)
    }(url)
}

wg.Wait() // blocks until all goroutines call Done()
```

## Context

`context.Context` carries deadlines, cancellation signals, and values across goroutines:

```go
ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
defer cancel()

req, _ := http.NewRequestWithContext(ctx, "GET", url, nil)
resp, err := http.DefaultClient.Do(req)
if err != nil {
    // could be context.DeadlineExceeded
    log.Fatal(err)
}
```

Every long-running or I/O operation should accept a `context.Context` as its first parameter:

```go
func (s *Service) GetUser(ctx context.Context, id string) (*User, error) {
    return s.db.QueryRowContext(ctx, "SELECT ... WHERE id = $1", id).Scan(...)
}
```

> **Important for HTTP servers:** Each incoming request has a context (`r.Context()`) that is canceled when the client disconnects. Always propagate it.
