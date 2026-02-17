---
title: "Networking & Volumes"
type: lesson
estimated_duration: "20m"
---

# Networking & Volumes

## Docker Networking

### Default Network Behavior

When you run `docker compose up`, Compose creates a **default network** for your project. All services can reach each other by their **service name**.

```yaml
services:
  app:
    image: myapp
    # Can reach db at hostname "db" and cache at "cache"

  db:
    image: postgres:17-alpine
    # Can reach app at hostname "app"

  cache:
    image: redis:7-alpine
```

```bash
# From the app container:
ping db        # resolves to the db container's IP
curl cache:6379  # connects to Redis
```

### Network Types

| Type | Description | Use Case |
|------|-------------|----------|
| `bridge` | Default, isolated network | Most applications |
| `host` | Share host network stack | Performance-critical apps |
| `none` | No networking | Batch jobs, security |
| `overlay` | Multi-host networking | Docker Swarm |

### Custom Networks

```yaml
services:
  app:
    networks:
      - frontend
      - backend

  db:
    networks:
      - backend

  nginx:
    networks:
      - frontend

networks:
  frontend:
  backend:
```

This isolates `db` from `nginx` — the database is only reachable from `app`.

### Exposing Ports

```yaml
services:
  app:
    ports:
      - "8080:8080"      # host:container — accessible from outside
    expose:
      - "9090"           # Only accessible from other containers
```

## Docker Volumes

Containers are **ephemeral** — when removed, their data is lost. Volumes persist data beyond the container lifecycle.

### Volume Types

#### Named Volumes (managed by Docker)

```yaml
services:
  db:
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:  # Docker manages the storage location
```

#### Bind Mounts (host directory)

```yaml
services:
  app:
    volumes:
      - ./src:/app/src         # Development: live code reload
      - ./config:/app/config:ro  # Read-only config mount
```

#### tmpfs (in-memory)

```yaml
services:
  app:
    tmpfs:
      - /tmp
      - /app/cache
```

### Volume Commands

```bash
# List volumes
docker volume ls

# Inspect a volume
docker volume inspect myproject_pgdata

# Remove unused volumes
docker volume prune

# Backup a volume
docker run --rm -v pgdata:/data -v $(pwd):/backup alpine \
    tar czf /backup/pgdata-backup.tar.gz -C /data .

# Restore a volume
docker run --rm -v pgdata:/data -v $(pwd):/backup alpine \
    tar xzf /backup/pgdata-backup.tar.gz -C /data
```

## Best Practices

1. **Use named volumes** for databases — don't bind-mount production data
2. **Use bind mounts** for development — live reload without rebuilds
3. **Mount configs read-only** — `:ro` prevents accidental modifications
4. **Use custom networks** to isolate services — not everything should talk to everything
5. **Never expose database ports** to the host in production

## Summary

Networking lets containers communicate securely. Volumes let data survive container restarts. Together, they form the foundation of multi-container applications.
