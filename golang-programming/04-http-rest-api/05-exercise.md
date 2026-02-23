---
title: "Build a REST Endpoint"
type: terminal-exercise
estimated_duration: "15m"
---

# Build a REST Endpoint

Practice creating and testing an HTTP server with Go.

## Step 1: Initialize the project

Create a new project for a simple API.

```console
$ ▌
```

- [x] `mkdir taskapi && cd taskapi && go mod init taskapi` — Creates the project and initializes the module.
- [ ] `go new taskapi` — There is no `go new` command.
- [ ] `go init taskapi` — The command is `go mod init`.

```output
go: creating new go.mod: module taskapi
```

## Step 2: Create and run the server

Create a minimal HTTP server and start it.

```console
$ ▌
```

- [x] `cat > main.go << 'EOF'
package main

import (
    "encoding/json"
    "log"
    "net/http"
)

func main() {
    mux := http.NewServeMux()
    mux.HandleFunc("GET /api/health", func(w http.ResponseWriter, r *http.Request) {
        w.Header().Set("Content-Type", "application/json")
        json.NewEncoder(w).Encode(map[string]string{"status": "ok"})
    })
    log.Println("Listening on :8080")
    log.Fatal(http.ListenAndServe(":8080", mux))
}
EOF
go run main.go &` — Creates a health endpoint and starts the server in background.
- [ ] `go start main.go` — There is no `go start` command.
- [ ] `go serve main.go` — There is no `go serve` command.

```output
Listening on :8080
```

## Step 3: Test the endpoint

Verify the API responds correctly using curl.

```console
$ ▌
```

- [x] `curl -s http://localhost:8080/api/health | python3 -m json.tool` — Calls the health endpoint and pretty-prints the JSON.
- [ ] `wget http://localhost:8080/api/health` — Works but saves to a file instead of displaying.
- [ ] `http GET localhost:8080/api/health` — Requires httpie to be installed.

```output
{
    "status": "ok"
}
```
