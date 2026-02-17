---
title: "SSH Tunneling"
type: lesson
estimated_duration: "15m"
---

# SSH Tunneling

SSH tunnels let you securely access services that aren't directly reachable. This is one of the most powerful tools in a DevOps engineer's toolkit.

## Local Port Forwarding

Access a remote service through a local port:

```bash
ssh -L 8080:localhost:3000 staging
```

This makes the staging server's port 3000 available at `localhost:8080` on your machine.

**Use cases:**
- Access a database that only accepts connections from the server
- View a web dashboard running on a private network
- Debug a service not exposed to the internet

### Example: Access a remote PostgreSQL database

```bash
ssh -L 5432:db.internal:5432 bastion
# Now connect: psql -h localhost -p 5432 -U admin production_db
```

## Remote Port Forwarding

Expose a local service to the remote server:

```bash
ssh -R 8080:localhost:3000 remote-server
```

Now `remote-server:8080` points to your local port 3000.

**Use cases:**
- Share your local development server with a teammate
- Expose a webhook endpoint during development
- Demo a local application without deploying

## Dynamic Port Forwarding (SOCKS Proxy)

Create a SOCKS proxy that routes all traffic through the SSH tunnel:

```bash
ssh -D 1080 bastion
```

Configure your browser or application to use `localhost:1080` as a SOCKS5 proxy. All traffic will be routed through the bastion server.

**Use cases:**
- Access multiple internal services without creating individual tunnels
- Browse internal documentation sites
- Test geo-restricted services

## Jump Hosts (ProxyJump)

Connect to a host through one or more intermediate hosts:

```bash
# Direct jump
ssh -J bastion internal-server

# Multi-hop
ssh -J bastion,dmz-server database-server
```

Or in your SSH config:

```
Host database
    HostName db.internal.company.com
    ProxyJump bastion,dmz
```

## Persistent Tunnels with autossh

For tunnels that need to survive network interruptions:

```bash
autossh -M 0 -f -N -L 5432:db.internal:5432 bastion
```

- `-M 0` — use SSH's built-in keep-alive
- `-f` — background the process
- `-N` — don't execute a remote command

## Summary

SSH tunnels are your Swiss Army knife for accessing services in complex network topologies. Master them and you'll rarely need a VPN for development work.
