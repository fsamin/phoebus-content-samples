---
title: "HTTP Quiz"
type: quiz
estimated_duration: "8m"
---

# HTTP Quiz

## [multiple-choice] What HTTP method is used to retrieve data?

- [x] GET
- [ ] POST
- [ ] PUT
- [ ] DELETE

> **Explanation:** GET requests data from the server without modifying anything. It should be safe and idempotent — calling it multiple times produces the same result.

## [multiple-choice] What does a 404 status code mean?

- [ ] The server crashed
- [ ] The request was unauthorized
- [x] The requested resource was not found
- [ ] The request was successful

> **Explanation:** 404 Not Found means the server understood the request but the resource doesn't exist. It's a client error (4xx), not a server error (5xx).

## [multiple-choice] Which status code range indicates a server error?

- [ ] 2xx
- [ ] 3xx
- [ ] 4xx
- [x] 5xx

> **Explanation:** 5xx codes indicate that the server failed to fulfill a valid request. Examples: 500 (Internal Server Error), 502 (Bad Gateway), 503 (Service Unavailable).

## [short-answer] What HTTP status code means "Created"?

    201

> **Explanation:** 201 Created indicates that a new resource was successfully created, typically in response to a POST request. The response usually includes the created resource.

## [multiple-choice] What is REST?

- [ ] A programming language for web servers
- [ ] A specific HTTP version
- [x] A set of design conventions for building APIs using HTTP methods and resource URLs
- [ ] A type of database

> **Explanation:** REST (Representational State Transfer) is a convention where resources are identified by URLs, actions are HTTP methods (GET, POST, PUT, DELETE), and responses use standard status codes.

## [multiple-choice] What does the `Content-Type: application/json` header indicate?

- [ ] The request requires authentication
- [ ] The response should be cached
- [x] The body of the message is formatted as JSON
- [ ] The server supports JSON but prefers XML

> **Explanation:** The Content-Type header tells the recipient the format of the body. `application/json` means the body is JSON, `text/html` means HTML, etc.

## [multiple-choice] What does "idempotent" mean for an HTTP method?

- [ ] The method is faster than others
- [ ] The method requires authentication
- [x] Calling it multiple times produces the same result as calling it once
- [ ] The method sends encrypted data

> **Explanation:** GET, PUT, and DELETE are idempotent: repeating the same request doesn't change the outcome. POST is not idempotent — posting the same data twice may create two resources.
