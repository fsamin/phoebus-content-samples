---
title: "Docker Compose Quiz"
type: quiz
estimated_duration: "10m"
---

# Docker Compose Quiz

## [multiple-choice] How do containers in the same Docker Compose project communicate?

- [ ] By IP address only
- [x] By service name — Compose creates a DNS entry for each service
- [ ] By container ID
- [ ] They can't communicate without explicit configuration

> **Explanation:** Docker Compose automatically creates a network for the project and registers each service name as a DNS entry. So `app` can reach `db` simply by using `db` as the hostname.

## [multiple-choice] What is the difference between `ports` and `expose` in a Compose file?

- [x] `ports` maps to the host (external access); `expose` is container-to-container only
- [ ] `ports` is for TCP; `expose` is for UDP
- [ ] `expose` is deprecated
- [ ] There is no difference

> **Explanation:** `ports: "8080:8080"` makes the service accessible from the host machine. `expose: "9090"` only documents the port and makes it reachable from other containers — not from the host.

## [short-answer] What Docker Compose command removes all containers, networks, AND volumes?

    docker compose down -v

> **Explanation:** `docker compose down` removes containers and networks. Adding `-v` also removes named volumes. This is useful for a clean reset but dangerous in production — you'll lose all data.

## [multiple-choice] Why should databases use named volumes instead of bind mounts?

- [ ] Named volumes are faster
- [x] Named volumes are managed by Docker, portable, and have correct permissions
- [ ] Bind mounts don't support PostgreSQL
- [ ] Named volumes are encrypted

> **Explanation:** Named volumes are managed by Docker, which handles permissions and portability. Bind mounts can have UID/GID mismatches between host and container, especially on Linux, causing permission errors with databases.

## [multiple-choice] What does `depends_on` with `condition: service_healthy` do?

- [ ] Starts the dependent service only on healthy hosts
- [ ] Checks if the Docker daemon is healthy
- [x] Waits for the dependency's health check to pass before starting the dependent service
- [ ] Restarts the dependent service if the dependency becomes unhealthy

> **Explanation:** With `condition: service_healthy`, Compose waits for the dependency's HEALTHCHECK to report healthy before starting the dependent service. This prevents the app from crashing because the database isn't ready yet.
