---
title: "What is SSH Quiz"
type: quiz
estimated_duration: "8m"
---

# What is SSH Quiz

## [multiple-choice] What was the main problem with Telnet that SSH solves?

- [ ] Telnet was too slow
- [x] Telnet sends everything in plain text, including passwords
- [ ] Telnet only worked on Windows
- [ ] Telnet didn't support file transfers

> **Explanation:** Telnet transmits all data — including credentials — as unencrypted plain text. Anyone intercepting the network traffic can read passwords directly. SSH encrypts everything.

## [multiple-choice] What year was SSH created?

- [ ] 1989
- [x] 1995
- [ ] 2001
- [ ] 2010

> **Explanation:** Tatu Ylönen created SSH in 1995 after a password-sniffing attack at his university in Finland. It quickly became the standard for secure remote access.

## [multiple-choice] What are the three main services SSH provides?

- [ ] Web hosting, email, and DNS
- [x] Encrypted remote shell, secure file transfer, and secure tunneling
- [ ] Firewall, antivirus, and backup
- [ ] Database access, logging, and monitoring

> **Explanation:** SSH provides an encrypted command-line shell on remote machines, secure file transfer (scp, sftp, rsync), and encrypted tunnels for forwarding network traffic.

## [short-answer] What is the default TCP port for SSH?

    22

> **Explanation:** SSH servers (sshd) listen on port 22 by default. This can be changed in the server configuration, and is sometimes changed to reduce automated attack noise.

## [multiple-choice] Why is public key authentication better than passwords?

- [ ] It is faster to type
- [ ] It works without an internet connection
- [x] The private key never leaves your machine, is virtually impossible to brute-force, and enables automation
- [ ] It doesn't require any setup

> **Explanation:** With key authentication, the private key stays on your machine. The server sends a challenge; your client solves it without revealing the key. Keys are thousands of bits long, making brute-force practically impossible.

## [multiple-choice] What is the most widely used SSH implementation?

- [ ] PuTTY
- [x] OpenSSH
- [ ] Bitvise SSH
- [ ] Dropbear

> **Explanation:** OpenSSH, maintained by the OpenBSD project, is the default SSH implementation on Linux, macOS, and Windows 10+. PuTTY is a popular Windows client, but OpenSSH is the standard.
