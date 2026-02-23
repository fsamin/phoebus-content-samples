---
title: "Go Fundamentals Quiz"
type: quiz
estimated_duration: "10m"
---

# Go Fundamentals Quiz

## [multiple-choice] When should you use a pointer receiver on a method?

- [ ] Always, for consistency
- [x] When the method needs to modify the receiver
- [ ] Only for exported methods
- [ ] Never, value receivers are always preferred

> **Explanation:** Use a pointer receiver `(t *Type)` when the method needs to modify the struct, or when the struct is large and copying would be expensive. Use a value receiver for small, immutable types.

## [multiple-choice] How does Go determine if a type implements an interface?

- [ ] The type must declare `implements InterfaceName`
- [ ] The type must be registered in an interface registry
- [x] Implicitly — if the type has all the interface's methods, it implements it
- [ ] The interface must list all implementing types

> **Explanation:** Go interfaces are satisfied implicitly. If a type has all the methods declared in an interface, it implements that interface automatically. This is called "structural typing" or "duck typing".

## [short-answer] What verb do you use with `fmt.Errorf` to wrap an error?

    %w

> **Explanation:** `fmt.Errorf("context: %w", err)` wraps the original error. The caller can then use `errors.Is()` or `errors.As()` to inspect the error chain. Using `%v` instead would format the error as a string without preserving the chain.

## [multiple-choice] What does `defer` do?

- [ ] Runs a function immediately in a new goroutine
- [ ] Delays execution for a specified duration
- [x] Schedules a function call to run when the enclosing function returns
- [ ] Retries a function call on error

> **Explanation:** `defer` is used for cleanup: closing files, releasing locks, recovering from panics. Deferred calls are executed in LIFO (last in, first out) order when the surrounding function returns.

## [multiple-choice] What is the purpose of `context.Context` in Go?

- [ ] To store global application configuration
- [ ] To manage database connection pools
- [x] To carry deadlines, cancellation signals, and request-scoped values
- [ ] To configure logging levels

> **Explanation:** `context.Context` is the standard way to propagate cancellation, timeouts, and request-scoped data. Every HTTP request has a context, and it should be passed to all downstream I/O operations.

## [multiple-choice] What happens when you send to an unbuffered channel with no receiver?

- [ ] The value is discarded
- [ ] A runtime error occurs
- [x] The sending goroutine blocks until a receiver is ready
- [ ] The channel automatically buffers the value

> **Explanation:** Unbuffered channels synchronize sender and receiver. A send blocks until another goroutine receives, and vice versa. This is the fundamental synchronization primitive in Go.
