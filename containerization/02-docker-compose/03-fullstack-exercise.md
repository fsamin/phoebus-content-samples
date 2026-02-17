---
title: "Build a Full Stack"
type: terminal-exercise
estimated_duration: "10m"
---

# Build a Full Stack with Docker Compose

You need to set up a complete development environment: a Node.js API, PostgreSQL database, and Redis cache.

## Step 1: Start the stack

All services are defined in `compose.yaml`. Start them in the background.

```console
$ ▌
```

- [ ] `docker compose up` — Starts in the foreground, blocks your terminal.
- [x] `docker compose up -d` — Starts all services in detached (background) mode.
- [ ] `docker run compose` — Not a valid Docker command.

```output
[+] Running 4/4
 ✔ Network myapp_default  Created
 ✔ Container myapp-cache  Started
 ✔ Container myapp-db     Started
 ✔ Container myapp-app    Started
```

## Step 2: Check that all services are healthy

Verify that the database is ready and the app is running.

```console
$ ▌
```

- [x] `docker compose ps` — Shows the status of all services including health.
- [ ] `docker ps -a` — Shows all Docker containers but not Compose-specific info.
- [ ] `docker compose logs` — Shows logs but not health status.

```output
NAME           SERVICE   STATUS          PORTS
myapp-app      app       Up 30 seconds   0.0.0.0:8080->8080/tcp
myapp-db       db        Up 30 seconds (healthy)   5432/tcp
myapp-cache    cache     Up 30 seconds   6379/tcp
```

## Step 3: View the application logs

Something seems wrong with the app. Check the logs.

```console
$ ▌
```

- [ ] `docker compose logs` — Shows all service logs, too noisy.
- [x] `docker compose logs app` — Shows only the application logs.
- [ ] `cat /var/log/app.log` — Logs are inside the container, not on the host.

```output
myapp-app  | Connected to PostgreSQL at db:5432
myapp-app  | Connected to Redis at cache:6379
myapp-app  | Server listening on :8080
```
