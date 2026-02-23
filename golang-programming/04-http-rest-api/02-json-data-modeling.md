---
title: "JSON and Data Modeling"
type: lesson
estimated_duration: "20m"
---

# JSON and Data Modeling

## Struct Tags

Go uses struct tags to control JSON serialization:

```go
type User struct {
    ID        int       `json:"id"`
    Username  string    `json:"username"`
    Email     string    `json:"email"`
    Password  string    `json:"-"`                    // never serialized
    CreatedAt time.Time `json:"created_at"`
    DeletedAt *time.Time `json:"deleted_at,omitempty"` // omitted if nil
}
```

## Encoding (struct → JSON)

```go
func respondJSON(w http.ResponseWriter, status int, data any) {
    w.Header().Set("Content-Type", "application/json")
    w.WriteHeader(status)
    if err := json.NewEncoder(w).Encode(data); err != nil {
        log.Printf("json encode error: %v", err)
    }
}

// Usage
respondJSON(w, http.StatusOK, user)
respondJSON(w, http.StatusCreated, map[string]string{"id": "123"})
```

## Decoding (JSON → struct)

```go
type CreateUserRequest struct {
    Username string `json:"username"`
    Email    string `json:"email"`
    Password string `json:"password"`
}

func decodeJSON(r *http.Request, dst any) error {
    dec := json.NewDecoder(r.Body)
    dec.DisallowUnknownFields() // reject unexpected fields
    if err := dec.Decode(dst); err != nil {
        return fmt.Errorf("invalid JSON: %w", err)
    }
    return nil
}

func createUser(w http.ResponseWriter, r *http.Request) {
    var req CreateUserRequest
    if err := decodeJSON(r, &req); err != nil {
        respondJSON(w, http.StatusBadRequest, map[string]string{"error": err.Error()})
        return
    }

    if req.Username == "" || req.Email == "" {
        respondJSON(w, http.StatusBadRequest, map[string]string{"error": "username and email required"})
        return
    }

    // create user...
}
```

## Response Patterns

### Success Responses

```go
// Single resource
respondJSON(w, http.StatusOK, user)

// Collection
respondJSON(w, http.StatusOK, map[string]any{
    "data":  users,
    "total": len(users),
})

// Created
respondJSON(w, http.StatusCreated, user)

// No content
w.WriteHeader(http.StatusNoContent)
```

### Error Responses

Use a consistent error format:

```go
type APIError struct {
    Error   string `json:"error"`
    Code    string `json:"code,omitempty"`
    Details any    `json:"details,omitempty"`
}

func respondError(w http.ResponseWriter, status int, msg string) {
    respondJSON(w, status, APIError{Error: msg})
}
```
