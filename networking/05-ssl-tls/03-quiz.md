---
title: "SSL/TLS Quiz"
type: quiz
estimated_duration: "8m"
---

# SSL/TLS Quiz

## [multiple-choice] What is the difference between SSL and TLS?

- [ ] SSL is for web browsers, TLS is for email
- [ ] SSL is open-source, TLS is commercial
- [x] TLS is the modern successor to SSL — all SSL versions are deprecated and insecure
- [ ] There is no difference, they are the same protocol

> **Explanation:** SSL was created by Netscape in 1995 and all versions (SSL 2.0, 3.0) are now considered insecure. TLS replaced SSL and is the current standard. TLS 1.3 (2018) is the latest version.

## [multiple-choice] What is the main difference between symmetric and asymmetric encryption?

- [x] Symmetric uses one shared key; asymmetric uses a public/private key pair
- [ ] Symmetric is more secure than asymmetric
- [ ] Asymmetric is faster than symmetric
- [ ] Symmetric only works with TLS 1.3

> **Explanation:** Symmetric encryption uses the same key for both encryption and decryption (fast, but key-sharing is hard). Asymmetric uses a key pair: public key to encrypt, private key to decrypt (slower, but solves key-sharing).

## [multiple-choice] How does TLS combine symmetric and asymmetric encryption?

- [ ] It uses only symmetric encryption
- [ ] It uses only asymmetric encryption
- [x] Asymmetric encryption exchanges a key, then symmetric encryption is used for data
- [ ] It alternates between both methods

> **Explanation:** TLS uses the hybrid approach: asymmetric encryption (slow but secure) to exchange a shared secret during the handshake, then symmetric encryption (fast) for all subsequent data.

## [multiple-choice] What does a TLS certificate prove?

- [ ] That the data is encrypted
- [ ] That the server is fast
- [x] That the server is who it claims to be, verified by a trusted Certificate Authority
- [ ] That the connection uses HTTP/2

> **Explanation:** Certificates prove identity. A Certificate Authority verifies that the server owns the domain and signs the certificate. Your browser checks this signature against its list of trusted CAs.

## [short-answer] What is the name of the free Certificate Authority that made HTTPS accessible to everyone?

    let's encrypt

> **Explanation:** Let's Encrypt, launched in 2016, provides free, automated TLS certificates. It uses the ACME protocol and tools like certbot for automatic issuance and renewal.

## [multiple-choice] What is HTTPS?

- [ ] A completely different protocol from HTTP
- [ ] HTTP with compression enabled
- [x] HTTP wrapped in a TLS encrypted tunnel
- [ ] HTTP version 2

> **Explanation:** HTTPS is simply HTTP over TLS. The HTTP protocol works identically, but all data is encrypted in transit, preventing eavesdropping and tampering.

## [multiple-choice] What happens if a TLS certificate is expired?

- [ ] The connection works normally
- [ ] The server refuses to respond
- [x] The browser shows a security warning and may block the connection
- [ ] The data is sent unencrypted instead

> **Explanation:** Expired certificates break the chain of trust. Browsers display a warning page and may prevent the user from continuing. This is why auto-renewal (via Let's Encrypt/certbot) is essential.
