---
title: "Secrets Management and Secure Configuration"
type: lesson
estimated_duration: "20m"
---

# Secrets Management and Secure Configuration

## The Golden Rule

> **Never hardcode secrets in source code.** Not in Go files, not in config files, not in Dockerfiles.

## Environment Variables

The simplest approach for secrets:

```go
dbPassword := os.Getenv("DB_PASSWORD")
if dbPassword == "" {
    log.Fatal("DB_PASSWORD environment variable is required")
}

dsn := fmt.Sprintf("postgres://app:%s@localhost:5432/mydb?sslmode=disable", dbPassword)
```

### Best Practices with Environment Variables

```bash
# ✅ Use a .env file for local development (add to .gitignore!)
echo "DB_PASSWORD=localdev123" > .env
echo ".env" >> .gitignore

# ✅ In production, inject via orchestrator
# Docker: docker run -e DB_PASSWORD=xxx
# Kubernetes: secretKeyRef in pod spec
# CI/CD: GitHub Actions secrets
```

## Secure Configuration Struct

```go
type Config struct {
    Database DatabaseConfig
    Auth     AuthConfig
}

type DatabaseConfig struct {
    Host     string `env:"DB_HOST" default:"localhost"`
    Port     int    `env:"DB_PORT" default:"5432"`
    Name     string `env:"DB_NAME" default:"app"`
    User     string `env:"DB_USER" default:"app"`
    Password string `env:"DB_PASSWORD" required:"true"` // no default!
    SSLMode  string `env:"DB_SSLMODE" default:"require"`
}

type AuthConfig struct {
    JWTSecret    string `env:"JWT_SECRET" required:"true"`
    BcryptCost   int    `env:"BCRYPT_COST" default:"12"`
}
```

## Encrypted Secrets at Rest

When storing secrets in a database (e.g., repository credentials):

```go
import "crypto/aes"
import "crypto/cipher"

func encrypt(plaintext []byte, key []byte) ([]byte, error) {
    block, err := aes.NewCipher(key)
    if err != nil {
        return nil, err
    }
    aesGCM, err := cipher.NewGCM(block)
    if err != nil {
        return nil, err
    }
    nonce := make([]byte, aesGCM.NonceSize())
    if _, err := io.ReadFull(rand.Reader, nonce); err != nil {
        return nil, err
    }
    return aesGCM.Seal(nonce, nonce, plaintext, nil), nil
}
```

> **AES-256-GCM** is the recommended algorithm: 256-bit key, authenticated encryption (integrity + confidentiality).

## Password Hashing

```go
import "golang.org/x/crypto/bcrypt"

// Hash a password (during registration)
hash, err := bcrypt.GenerateFromPassword([]byte(password), bcrypt.DefaultCost)

// Verify a password (during login)
err := bcrypt.CompareHashAndPassword(hash, []byte(password))
if err != nil {
    // password does not match
}
```

> **Never** store passwords in plain text. **Never** use MD5 or SHA for passwords (no salt, too fast to brute-force). Use `bcrypt`, `scrypt`, or `argon2`.

## HTTP Security Headers

```go
func securityHeaders(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        w.Header().Set("X-Content-Type-Options", "nosniff")
        w.Header().Set("X-Frame-Options", "DENY")
        w.Header().Set("Content-Security-Policy", "default-src 'self'")
        w.Header().Set("Strict-Transport-Security", "max-age=31536000; includeSubDomains")
        w.Header().Set("Referrer-Policy", "strict-origin-when-cross-origin")
        next.ServeHTTP(w, r)
    })
}
```
