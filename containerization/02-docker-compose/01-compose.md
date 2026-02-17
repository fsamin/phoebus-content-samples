---
title: "Docker Compose"
type: lesson
estimated_duration: "25m"
---

# Docker Compose

## What is Docker Compose?

Docker Compose is a tool for defining and running **multi-container applications**. Instead of running multiple `docker run` commands, you describe your entire stack in a single YAML file.

## The compose.yaml File

```yaml
services:
  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      - DATABASE_URL=postgres://user:pass@db:5432/myapp
      - REDIS_URL=redis://cache:6379
    depends_on:
      db:
        condition: service_healthy
      cache:
        condition: service_started
    restart: unless-stopped

  db:
    image: postgres:17-alpine
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: myapp
    volumes:
      - pgdata:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user -d myapp"]
      interval: 5s
      timeout: 5s
      retries: 5

  cache:
    image: redis:7-alpine
    volumes:
      - redisdata:/data

volumes:
  pgdata:
  redisdata:
```

## Essential Commands

```bash
# Start all services
docker compose up -d

# View logs
docker compose logs -f
docker compose logs app   # specific service

# Stop all services
docker compose down

# Stop and remove volumes (careful!)
docker compose down -v

# Rebuild images
docker compose build
docker compose up -d --build

# Scale a service
docker compose up -d --scale worker=3

# Execute command in a service
docker compose exec app /bin/sh

# View running services
docker compose ps
```

## Service Configuration

### Build from Dockerfile

```yaml
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
      args:
        VERSION: "1.0"
```

### Environment Variables

```yaml
services:
  app:
    # Inline
    environment:
      - NODE_ENV=production
      - API_KEY=secret

    # From file
    env_file:
      - .env
      - .env.production
```

### Health Checks

```yaml
services:
  app:
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8080/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 10s
```

### Resource Limits

```yaml
services:
  app:
    deploy:
      resources:
        limits:
          cpus: "0.50"
          memory: 512M
        reservations:
          cpus: "0.25"
          memory: 256M
```

## Profiles

Group services by use case:

```yaml
services:
  app:
    build: .

  db:
    image: postgres:17-alpine

  monitoring:
    image: grafana/grafana
    profiles: ["monitoring"]

  debug:
    image: busybox
    profiles: ["debug"]
```

```bash
docker compose up -d               # Only app + db
docker compose --profile monitoring up -d  # + monitoring
```

## Summary

Docker Compose is the standard for local development and simple production deployments. One file describes your entire stack — application, database, cache, monitoring. Master `compose.yaml` and you'll set up any development environment in seconds.
