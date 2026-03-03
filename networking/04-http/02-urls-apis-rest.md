---
title: "URLs, APIs, and REST"
type: lesson
estimated_duration: "15m"
---

# URLs, APIs, and REST

## Anatomy of a URL

Every web resource is identified by a **URL** (Uniform Resource Locator):

```
https://api.example.com:8443/v2/users?role=admin&page=1#results
└─┬──┘ └──────┬───────┘└─┬─┘└───┬───┘└────────┬───────┘└──┬──┘
scheme       host      port   path         query       fragment
```

| Part | Description | Required? |
|------|-------------|-----------|
| **Scheme** | Protocol (`http`, `https`, `ftp`) | Yes |
| **Host** | Server name or IP | Yes |
| **Port** | Network port (default: 80 for HTTP, 443 for HTTPS) | No |
| **Path** | Resource location on the server | Yes (at least `/`) |
| **Query** | Parameters after `?`, separated by `&` | No |
| **Fragment** | Client-side anchor (not sent to server) | No |

## What is an API?

An **API** (Application Programming Interface) is a way for programs to communicate with each other over HTTP. Instead of returning HTML pages for humans, APIs return structured data (usually JSON) for programs.

```
# Browser request (returns HTML)
GET /users → <html><body>User list...</body></html>

# API request (returns JSON)
GET /api/users → [{"id": 1, "name": "Alice"}, {"id": 2, "name": "Bob"}]
```

APIs are how modern applications work:
- Your mobile app calls a backend API
- Your frontend JavaScript calls a backend API
- Microservices call each other's APIs
- Third-party integrations use APIs (Stripe, GitHub, Slack)

## REST: A Design Convention

**REST** (Representational State Transfer) is a set of conventions for designing APIs. It's not a protocol — it's a style guide.

REST principles:
1. **Resources** are identified by URLs (`/users`, `/users/42`)
2. **HTTP methods** define actions (`GET` = read, `POST` = create, etc.)
3. **Stateless** — Each request contains all needed information
4. **Standard status codes** communicate results

### REST in Practice

| Action | Method | URL | Body | Response |
|--------|--------|-----|------|----------|
| List all users | `GET` | `/api/users` | — | `200` + JSON array |
| Get one user | `GET` | `/api/users/42` | — | `200` + JSON object |
| Create a user | `POST` | `/api/users` | JSON | `201` + created object |
| Update a user | `PUT` | `/api/users/42` | JSON | `200` + updated object |
| Delete a user | `DELETE` | `/api/users/42` | — | `204` No Content |

### Example API Call with curl

```bash
# GET request
curl https://api.example.com/users

# POST request with JSON body
curl -X POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -d '{"name": "Alice", "email": "alice@example.com"}'

# GET with authentication
curl -H "Authorization: Bearer my-token" \
  https://api.example.com/users/42
```

`curl` is the Swiss Army knife for working with HTTP. You'll use it daily as a DevOps engineer to test APIs, debug services, and check endpoints.
