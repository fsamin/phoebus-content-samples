---
title: "DNS Record Types"
type: lesson
estimated_duration: "15m"
---

# DNS Record Types

DNS doesn't just map names to IP addresses. It stores different types of **records**, each serving a specific purpose.

## The Essential Record Types

### A Record (Address)

Maps a domain name to an **IPv4 address**. This is the most common record type.

```
example.com.    A    93.184.216.34
```

### AAAA Record (IPv6 Address)

Maps a domain name to an **IPv6 address**. Same as A, but for IPv6.

```
example.com.    AAAA    2606:2800:220:1:248:1893:25c8:1946
```

### CNAME Record (Canonical Name)

Creates an **alias** — points one domain name to another.

```
www.example.com.    CNAME    example.com.
blog.example.com.   CNAME    mysite.wordpress.com.
```

When you look up `www.example.com`, DNS first finds the CNAME (`example.com`), then looks up that domain's A record. CNAMEs are useful for pointing subdomains to external services.

> ⚠️ **A CNAME cannot coexist with other records on the same name.** You cannot have both a CNAME and an A record for `example.com`.

### MX Record (Mail Exchange)

Specifies which servers handle **email** for the domain. Includes a priority number (lower = preferred).

```
example.com.    MX    10    mail1.example.com.
example.com.    MX    20    mail2.example.com.
```

Email to `user@example.com` is first sent to `mail1` (priority 10). If it's unavailable, `mail2` (priority 20) is tried.

### TXT Record (Text)

Stores arbitrary text. Used for domain verification, email security, and more.

```
example.com.    TXT    "v=spf1 include:_spf.google.com ~all"
```

Common uses:
- **SPF** — Which servers can send email for this domain
- **DKIM** — Email signing verification
- **Domain verification** — Prove you own the domain (Google, Let's Encrypt, etc.)

### NS Record (Name Server)

Declares which servers are **authoritative** for this domain.

```
example.com.    NS    ns1.example.com.
example.com.    NS    ns2.example.com.
```

### SRV Record (Service)

Specifies the host and port for specific services. Used by some protocols to discover services automatically.

```
_sip._tcp.example.com.    SRV    10 5 5060 sipserver.example.com.
```

### PTR Record (Pointer — Reverse DNS)

Maps an **IP address back to a domain name** — the reverse of an A record. Used for reverse DNS lookups, often for email verification.

```
34.216.184.93.in-addr.arpa.    PTR    example.com.
```

## Quick Reference

| Record | Purpose | Example Value |
|--------|---------|--------------|
| **A** | Name → IPv4 | `93.184.216.34` |
| **AAAA** | Name → IPv6 | `2606:2800:220:1::` |
| **CNAME** | Name → another name | `example.com.` |
| **MX** | Mail server | `10 mail.example.com.` |
| **TXT** | Text data (SPF, DKIM) | `"v=spf1 ..."` |
| **NS** | Authoritative name servers | `ns1.example.com.` |
| **SRV** | Service location + port | `10 5 5060 host.` |
| **PTR** | IP → name (reverse) | `example.com.` |

## Real-World Example

Here's what the DNS records for a typical web application might look like:

```
; Web server
example.com.        A       203.0.113.10
www.example.com.    CNAME   example.com.

; API on a cloud provider
api.example.com.    CNAME   my-api.us-east-1.elb.amazonaws.com.

; Email
example.com.        MX      10  mx1.mail.example.com.
example.com.        MX      20  mx2.mail.example.com.

; Email security
example.com.        TXT     "v=spf1 include:_spf.google.com ~all"

; Name servers
example.com.        NS      ns1.example.com.
example.com.        NS      ns2.example.com.
```
