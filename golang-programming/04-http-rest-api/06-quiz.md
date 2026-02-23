---
title: "REST API Quiz"
type: quiz
estimated_duration: "10m"
---

# REST API Quiz

## [multiple-choice] What interface must a type implement to be an HTTP handler in Go?

- [ ] `http.Server`
- [x] `http.Handler` — with method `ServeHTTP(ResponseWriter, *Request)`
- [ ] `http.Router`
- [ ] `io.Handler`

> **Explanation:** The `http.Handler` interface has a single method: `ServeHTTP(http.ResponseWriter, *http.Request)`. The `http.HandlerFunc` type is an adapter that lets you use ordinary functions as handlers.

## [multiple-choice] What struct tag hides a field from JSON output?

- [ ] `json:"omitempty"`
- [ ] `json:"hidden"`
- [x] `json:"-"`
- [ ] `json:"private"`

> **Explanation:** The tag `json:"-"` tells `encoding/json` to skip the field during both encoding and decoding. This is commonly used for sensitive fields like passwords. `omitempty` only omits zero-valued fields, it doesn't hide them.

## [short-answer] What HTTP status code should a successful DELETE return when there is no response body?

    204

> **Explanation:** HTTP 204 No Content is the standard response for a successful DELETE that returns no body. The `w.WriteHeader(http.StatusNoContent)` call sends this status.

## [multiple-choice] What is the purpose of `sync.RWMutex` in the task store example?

- [ ] To limit the number of concurrent HTTP connections
- [ ] To encrypt data at rest
- [x] To allow concurrent reads while ensuring exclusive writes
- [ ] To queue database transactions

> **Explanation:** `RWMutex` allows multiple goroutines to read (`RLock`) simultaneously, but only one goroutine can write (`Lock`) at a time. This is essential for in-memory stores accessed by multiple HTTP handler goroutines.

## [multiple-choice] Why do we use `*string` (pointer) in `UpdateTaskRequest` instead of `string`?

- [ ] To save memory
- [ ] For compatibility with JSON libraries
- [x] To distinguish between "field not provided" (nil) and "field set to empty string" ("")
- [ ] Pointers are always faster in Go

> **Explanation:** With a plain `string`, an absent JSON field and `""` both result in the zero value `""`. With `*string`, an absent field is `nil` while an explicit `""` is a pointer to `""`. This enables proper PATCH semantics — only updating fields that were actually sent.

## [multiple-choice] What does `json.NewDecoder(r.Body).Decode(&req)` do?

- [ ] Writes JSON to the response body
- [x] Reads JSON from the request body and populates the struct
- [ ] Validates the JSON schema
- [ ] Converts the struct to a JSON string

> **Explanation:** `json.NewDecoder` creates a streaming JSON decoder from any `io.Reader` (here `r.Body`). `Decode(&req)` reads the JSON and populates the struct fields matching the `json` struct tags.
