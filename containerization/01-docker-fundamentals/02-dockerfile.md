---
title: "Writing Dockerfiles"
type: lesson
estimated_duration: "25m"
---

# Writing Dockerfiles

## Dockerfile Basics

A Dockerfile is a text file with instructions to build an image. Each instruction creates a layer.

### Simple example

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --production
COPY . .
EXPOSE 3000
CMD ["node", "server.js"]
```

## Key Instructions

| Instruction | Purpose | Example |
|-------------|---------|---------|
| `FROM` | Base image | `FROM golang:1.22-alpine` |
| `WORKDIR` | Set working directory | `WORKDIR /app` |
| `COPY` | Copy files from host | `COPY . .` |
| `ADD` | Copy + extract archives / fetch URLs | `ADD app.tar.gz /app` |
| `RUN` | Execute command during build | `RUN apt-get install -y curl` |
| `ENV` | Set environment variable | `ENV NODE_ENV=production` |
| `ARG` | Build-time variable | `ARG VERSION=1.0` |
| `EXPOSE` | Document port | `EXPOSE 8080` |
| `CMD` | Default command | `CMD ["./server"]` |
| `ENTRYPOINT` | Fixed command (args appended) | `ENTRYPOINT ["./server"]` |
| `HEALTHCHECK` | Container health check | `HEALTHCHECK CMD curl -f http://localhost/` |
| `USER` | Run as non-root user | `USER appuser` |

## Multi-stage Builds

Reduce final image size by separating build and runtime:

```dockerfile
# Stage 1: Build
FROM golang:1.22-alpine AS builder
WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o /app ./cmd/server

# Stage 2: Runtime
FROM alpine:3.19
RUN apk --no-cache add ca-certificates
COPY --from=builder /app /app
USER 65534:65534
EXPOSE 8080
CMD ["/app"]
```

Result: a tiny image with just the binary and CA certificates.

## Best Practices

### 1. Order layers by change frequency

```dockerfile
# Least changing → most changing
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./        # Changes rarely
RUN npm ci --production      # Cached if package.json unchanged
COPY . .                     # Changes often (source code)
```

### 2. Use specific image tags

```dockerfile
# Bad — could break any time
FROM node:latest

# Good — pinned and reproducible
FROM node:20.11-alpine3.19
```

### 3. Run as non-root

```dockerfile
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
```

### 4. Use .dockerignore

```dockerignore
.git
node_modules
*.md
.env
.vscode
Dockerfile
docker-compose.yaml
```

### 5. Combine RUN commands

```dockerfile
# Bad — 3 layers
RUN apt-get update
RUN apt-get install -y curl
RUN rm -rf /var/lib/apt/lists/*

# Good — 1 layer, cleanup included
RUN apt-get update && \
    apt-get install -y --no-install-recommends curl && \
    rm -rf /var/lib/apt/lists/*
```

### 6. Add health checks

```dockerfile
HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
    CMD wget --no-verbose --tries=1 --spider http://localhost:8080/health || exit 1
```

## Real-world Example: Go application

```dockerfile
FROM golang:1.22-alpine AS builder
RUN apk add --no-cache git
WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build \
    -ldflags="-s -w -X main.version=$(git describe --tags)" \
    -o /server ./cmd/server

FROM scratch
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=builder /server /server
EXPOSE 8080
ENTRYPOINT ["/server"]
```

This produces an image of ~10MB with just the binary.

## Summary

A well-written Dockerfile produces small, secure, fast-building images. Use multi-stage builds, pin versions, run as non-root, and order layers by change frequency. Your Dockerfile is infrastructure as code — treat it with the same care as your application code.
