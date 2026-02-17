---
title: "Fix the Dockerfile"
type: code-exercise
mode: identify-and-fix
estimated_duration: "10m"
target:
  file: "Dockerfile"
  lines: [3, 8]
---

# Fix the Dockerfile

A teammate wrote a Dockerfile for a Go application, but it has several issues that make the image insecure and bloated. The image is 1.2 GB instead of the expected ~15 MB.

Review the Dockerfile and identify the problems.

## Patches

### [x] Use multi-stage build and run as non-root

The Dockerfile should use a multi-stage build to separate compilation from runtime, and the final image should not run as root.

```diff
--- a/Dockerfile
+++ b/Dockerfile
@@ -1,10 +1,16 @@
-FROM golang:1.22
-WORKDIR /app
-COPY . .
-RUN go build -o server .
-EXPOSE 8080
-ENV GIN_MODE=release
-USER root
-CMD ["./server"]
+FROM golang:1.22-alpine AS builder
+WORKDIR /src
+COPY go.mod go.sum ./
+RUN go mod download
+COPY . .
+RUN CGO_ENABLED=0 go build -o /server .
+
+FROM alpine:3.19
+RUN addgroup -S app && adduser -S app -G app
+COPY --from=builder /server /server
+EXPOSE 8080
+ENV GIN_MODE=release
+USER app
+CMD ["/server"]
```

### [ ] Just change the USER directive

Changing `USER root` to `USER nobody` fixes the security issue but the image is still bloated because the entire Go toolchain is included.

```diff
--- a/Dockerfile
+++ b/Dockerfile
@@ -7,3 +7,3 @@
 ENV GIN_MODE=release
-USER root
+USER nobody
 CMD ["./server"]
```

### [ ] Add .dockerignore only

A `.dockerignore` file helps but doesn't solve the fundamental problem of including the Go toolchain in the final image.

```diff
--- a/Dockerfile
+++ b/Dockerfile
@@ -1,4 +1,4 @@
-FROM golang:1.22
+FROM golang:1.22-alpine
 WORKDIR /app
 COPY . .
 RUN go build -o server .
```
