---
title: "GPG Quiz"
type: quiz
estimated_duration: "10m"
---

# GPG Quiz

## [multiple-choice] What is the primary purpose of signing a Git commit with GPG?

- [ ] Encrypting the commit message
- [x] Proving the author's identity and commit integrity
- [ ] Compressing the commit data
- [ ] Speeding up the push operation

> **Explanation:** GPG signing creates a cryptographic signature that proves the commit was made by the holder of the private key and hasn't been tampered with. It doesn't encrypt or compress anything.

## [multiple-choice] Which GPG algorithm is recommended for new keys?

- [ ] RSA 2048
- [ ] DSA
- [x] Ed25519
- [ ] ElGamal

> **Explanation:** Ed25519 is the modern standard — it's faster, produces smaller signatures, and provides excellent security. RSA 2048 is too weak by current standards, DSA is deprecated, and ElGamal is encryption-only.

## [short-answer] What Git config command enables automatic GPG signing for all commits?

    git config --global commit.gpgsign true

> **Explanation:** Setting `commit.gpgsign` to `true` means every `git commit` will be signed automatically. You also need `user.signingkey` configured with your GPG key ID.

## [multiple-choice] What happens when you encrypt a file with `gpg --encrypt --recipient alice@company.com file.txt`?

- [ ] The file is signed with Alice's key
- [ ] The file is encrypted with a symmetric password
- [x] The file is encrypted so only Alice can decrypt it with her private key
- [ ] The file is compressed and archived

> **Explanation:** `--encrypt --recipient` uses Alice's public key to encrypt the file. Only Alice's matching private key can decrypt it. This is asymmetric encryption.

## [multiple-choice] Why should GPG keys always have an expiration date?

- [ ] GPG requires it
- [ ] Keys stop working without it
- [x] If a key is compromised, the damage is time-limited, and it forces regular key hygiene
- [ ] Expired keys are faster to verify

> **Explanation:** Expiration dates limit the damage window if a key is compromised and encourage regular key rotation. You can always extend expiration before it lapses if the key is still secure.
