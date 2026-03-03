---
title: "TLS and Certificates"
type: lesson
estimated_duration: "20m"
---

# TLS and Certificates

## SSL vs TLS

You'll hear both terms used interchangeably, but technically:

- **SSL** (Secure Sockets Layer) — The original protocol, created by Netscape in 1995. All versions are now **deprecated and insecure**.
- **TLS** (Transport Layer Security) — The successor to SSL. Current version: **TLS 1.3** (2018).

When people say "SSL certificate", they really mean "TLS certificate". The name stuck from the early days.

## What is HTTPS?

**HTTPS** = HTTP + TLS. It's the same HTTP protocol, but wrapped in a TLS encrypted tunnel.

```
HTTP:   Client ←──plain text──→ Server     (anyone can read)
HTTPS:  Client ←──encrypted──→ Server      (only client and server can read)
```

You can tell HTTPS is active by:
- The URL starts with `https://`
- Your browser shows a **padlock icon** 🔒

## The TLS Handshake (Simplified)

Before encrypted communication begins, the client and server perform a **handshake**:

```
Client                              Server
  │                                    │
  │──── ClientHello ─────────────────►│
  │     "I support TLS 1.3,           │
  │      these cipher suites"         │
  │                                    │
  │◄──── ServerHello ─────────────────│
  │      "Let's use TLS 1.3,          │
  │       this cipher suite"          │
  │      + Server Certificate         │
  │                                    │
  │  Client verifies certificate      │
  │  Both derive session keys         │
  │                                    │
  │◄═══ Encrypted communication ═════►│
```

In TLS 1.3, this takes only **1 round-trip** (vs 2 in TLS 1.2), making connections faster.

## Certificates: Proving Identity

Encryption alone isn't enough. If you encrypt your data and send it to an attacker pretending to be your bank, encryption doesn't help — you just securely sent your password to the wrong person.

**Certificates** solve this by proving that the server is who it claims to be.

### What's in a Certificate?

| Field | Description |
|-------|-------------|
| **Subject** | The domain name (`www.example.com`) |
| **Issuer** | The Certificate Authority that signed it |
| **Public key** | The server's public key |
| **Validity period** | Start and expiration dates |
| **Signature** | The CA's digital signature proving authenticity |

### Certificate Authorities (CAs)

A **Certificate Authority** is a trusted organization that verifies domain ownership and signs certificates. Your browser/OS comes with a list of trusted CAs.

```
Root CA (trusted by your browser)
    │
    └── Intermediate CA
            │
            └── example.com certificate
                (signed by Intermediate CA)
```

When your browser receives a certificate, it follows the **chain of trust**:
1. Is this certificate signed by a trusted CA? → Check
2. Is the domain name correct? → Check
3. Is it still valid (not expired)? → Check
4. Has it been revoked? → Check

If all checks pass → 🔒 secure connection. If any fail → ⚠️ warning.

### Let's Encrypt

**Let's Encrypt** is a free, automated Certificate Authority launched in 2016. It made HTTPS accessible to everyone by:

- **Free certificates** (no payment required)
- **Automated** issuance and renewal via the ACME protocol
- **90-day validity** (short-lived for security, auto-renewed)

Tools like **certbot** automate the entire process:

```bash
# Get a certificate for your domain
certbot certonly --nginx -d example.com -d www.example.com

# Certificates are automatically renewed
```

Before Let's Encrypt, certificates cost $50–$300/year. Today, there's no excuse for running a website without HTTPS.

## Why HTTPS Matters

1. **Privacy** — No one can read your data in transit
2. **Integrity** — No one can modify data without detection
3. **Authentication** — You know you're talking to the real server
4. **SEO** — Google ranks HTTPS sites higher
5. **Browser trust** — Modern browsers flag HTTP sites as "Not Secure"
6. **Required by modern APIs** — Service workers, geolocation, and many browser APIs require HTTPS
