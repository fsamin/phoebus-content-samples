---
title: "DNS Quiz"
type: quiz
estimated_duration: "8m"
---

# DNS Quiz

## [multiple-choice] What does DNS do?

- [ ] Encrypts web traffic
- [x] Translates domain names (like google.com) into IP addresses
- [ ] Assigns IP addresses to devices via DHCP
- [ ] Routes packets between networks

> **Explanation:** DNS (Domain Name System) maps human-readable domain names to IP addresses, allowing you to type `google.com` instead of `142.250.74.238`.

## [multiple-choice] What is a recursive DNS resolver?

- [ ] The server that owns the domain's DNS records
- [x] A server that follows the DNS hierarchy on behalf of clients, querying root, TLD, and authoritative servers
- [ ] A DNS server that only caches results
- [ ] A DNS client built into your web browser

> **Explanation:** The recursive resolver does the heavy lifting: it queries the root servers, then TLD servers, then authoritative servers to find the final IP address, and caches the result for future lookups.

## [short-answer] What DNS record type maps a domain name to an IPv4 address?

    A

> **Explanation:** An A record (Address record) maps a domain name to an IPv4 address. For IPv6, the equivalent is the AAAA record.

## [multiple-choice] What is a CNAME record?

- [ ] A record that stores the server's SSL certificate
- [ ] A record that maps a name to an IP address
- [x] An alias that points one domain name to another domain name
- [ ] A record that specifies mail servers

> **Explanation:** A CNAME (Canonical Name) creates an alias. For example, `www.example.com CNAME example.com` means "www is just another name for example.com."

## [multiple-choice] What does TTL mean in DNS?

- [ ] Total Transfer Limit
- [x] Time To Live — how long a DNS response can be cached before re-querying
- [ ] The Transfer Layer protocol
- [ ] Top-level TLD Lookup

> **Explanation:** TTL (Time To Live) is the number of seconds a DNS answer can be cached. A TTL of 300 means the result is valid for 5 minutes. After that, a new DNS query is needed.

## [multiple-choice] Which DNS record type specifies mail servers for a domain?

- [ ] A
- [ ] CNAME
- [ ] TXT
- [x] MX

> **Explanation:** MX (Mail Exchange) records specify which servers handle email for a domain, along with a priority number. Email clients query MX records to know where to deliver mail.

## [short-answer] What command-line tool is most commonly used to query DNS records?

    dig

> **Explanation:** `dig` (Domain Information Groper) is the standard tool for querying DNS. Example: `dig www.example.com` returns the A record, nameservers used, and TTL.
