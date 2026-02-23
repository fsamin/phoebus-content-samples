---
title: "Input Validation and Injection Prevention"
type: lesson
estimated_duration: "25m"
---

# Input Validation and Injection Prevention

## Why Validation Matters

Every piece of data from the outside world is untrusted: HTTP request bodies, query parameters, headers, file uploads, environment variables. Never trust user input.

## Input Validation Patterns

### Manual Validation

```go
type CreateUserRequest struct {
    Username string `json:"username"`
    Email    string `json:"email"`
    Password string `json:"password"`
}

func (r CreateUserRequest) Validate() error {
    if len(r.Username) < 3 || len(r.Username) > 50 {
        return fmt.Errorf("username must be between 3 and 50 characters")
    }
    if !strings.Contains(r.Email, "@") {
        return fmt.Errorf("invalid email address")
    }
    if len(r.Password) < 8 {
        return fmt.Errorf("password must be at least 8 characters")
    }
    return nil
}

func createUser(w http.ResponseWriter, r *http.Request) {
    var req CreateUserRequest
    if err := json.NewDecoder(r.Body).Decode(&req); err != nil {
        respondError(w, http.StatusBadRequest, "invalid JSON")
        return
    }
    if err := req.Validate(); err != nil {
        respondError(w, http.StatusBadRequest, err.Error())
        return
    }
    // safe to proceed
}
```

### Sanitizing Strings

```go
import "html"

// Prevent XSS in any user-provided text that might be rendered
safe := html.EscapeString(userInput)
```

### Limiting Request Body Size

```go
// Limit request body to 1MB
r.Body = http.MaxBytesReader(w, r.Body, 1<<20)
```

## SQL Injection Prevention

### ❌ Vulnerable — String Concatenation

```go
// NEVER DO THIS
query := "SELECT * FROM users WHERE username = '" + username + "'"
db.Query(query)
```

An attacker sending `admin' OR '1'='1` would bypass authentication.

### ✅ Safe — Parameterized Queries

```go
// Always use parameterized queries
row := db.QueryRow("SELECT id, username, email FROM users WHERE username = $1", username)

// With sqlx
var user User
err := db.Get(&user, "SELECT * FROM users WHERE id = $1", userID)

// INSERT
_, err := db.Exec(
    "INSERT INTO users (username, email, password_hash) VALUES ($1, $2, $3)",
    req.Username, req.Email, hashedPassword,
)
```

The database driver escapes parameters automatically. This works for **all** SQL operations.

### Parameterized Queries — How They Work

```
Application sends:  "SELECT * FROM users WHERE id = $1" + [42]
Database receives:  Query template + parameter list (separately)
Database executes:  The parameter is NEVER interpreted as SQL
```

## Command Injection Prevention

### ❌ Vulnerable

```go
// NEVER DO THIS
cmd := exec.Command("sh", "-c", "git clone " + userURL)
```

### ✅ Safe — Separate Arguments

```go
// Arguments are passed as separate strings, never through a shell
cmd := exec.Command("git", "clone", userURL)
```

## Path Traversal Prevention

### ❌ Vulnerable

```go
// NEVER DO THIS
http.ServeFile(w, r, "/uploads/" + r.URL.Query().Get("file"))
```

### ✅ Safe — Use `filepath.Clean` + Validate

```go
filename := filepath.Clean(r.URL.Query().Get("file"))
fullPath := filepath.Join("/uploads", filename)

// Ensure the resolved path is still under /uploads
if !strings.HasPrefix(fullPath, "/uploads/") {
    http.Error(w, "Forbidden", http.StatusForbidden)
    return
}
http.ServeFile(w, r, fullPath)
```
