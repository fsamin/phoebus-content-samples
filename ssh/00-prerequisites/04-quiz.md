---
title: "Prerequisites Quiz"
type: quiz
estimated_duration: "8m"
---

# Prerequisites Quiz

## [multiple-choice] Which command displays the directory you are currently in?

- [ ] `ls`
- [x] `pwd`
- [ ] `cd`
- [ ] `whoami`

> **Explanation:** The `pwd` command (*print working directory*) displays the full path of the current directory. It is very useful for knowing where you are in the file tree.

## [short-answer] What does `chmod 600` applied to a file mean?

    read and write for the owner only

> **Explanation:** The first digit `6` (4+2) grants read and write permissions to the owner. The two following `0`s mean that no permissions are granted to the group or other users. This is the recommended permission for private SSH keys.

## [multiple-choice] In a file path, what does the `~` symbol represent?

- [ ] The system root directory
- [ ] The previous directory
- [x] The user's home directory
- [ ] The temporary directory

> **Explanation:** The `~` symbol is a shortcut for your home directory. For example, `~/.ssh` corresponds to `/home/your_name/.ssh` on Linux. It is a very handy shortcut to know!

## [multiple-choice] What is a network port?

- [ ] A physical cable that connects two computers
- [ ] The IP address of a server
- [x] A number that identifies a specific service on a machine, like an apartment number in a building
- [ ] A network security software

> **Explanation:** If the IP address is the address of a building, the port is the apartment number. It directs traffic to the right service on the machine. For example, port 80 is used for the web (HTTP) and port 22 for SSH.

## [short-answer] What is the default port number used by SSH?

    22

> **Explanation:** Port 22 is the standard port assigned to the SSH protocol. SSH servers listen on this port by default, although it is possible to change it in the configuration.

## [multiple-choice] In the client/server model, who initiates the connection?

- [ ] The server
- [x] The client
- [ ] Both at the same time
- [ ] The router

> **Explanation:** It is always the client that initiates the connection to the server. The server patiently waits for connection requests. For example, when you connect via SSH to a remote machine, your computer is the client.

## [multiple-choice] In asymmetric encryption, which key can be shared freely?

- [ ] The private key
- [x] The public key
- [ ] Both keys
- [ ] Neither of them

> **Explanation:** The public key is designed to be shared with everyone, without risk. It is the private key that must remain secret and never leave your machine. Together, they form a complementary pair.

## [multiple-choice] What does a digital signature prove?

- [ ] That the message is encrypted
- [ ] That the message has not been read
- [x] The identity of the message sender
- [ ] The transmission speed of the message

> **Explanation:** A digital signature verifies that the message truly comes from the person who claims to have sent it. The sender signs with their private key, and anyone can verify this signature with the corresponding public key.
