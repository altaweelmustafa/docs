---
title: "Chapter 5, Part 1 – Multiple Encryption, Two-Key/Three-Key 3DES & Why Modes Exist"
date: 2026-08-23
description: "The two-key variant of Triple DES and the attacks against it, the three-key EDE variant, and why NIST defined five standard modes of operation for block ciphers."
tags: [information-security, cryptography, triple-des, modes-of-operation, chapter5]
toc: true
weight: 1
---

## Multiple encryption, revisited

Given plaintext `P` and two keys `K1`, `K2`, two-stage multiple encryption produces:

```
C = E(K2, E(K1, P))          Decryption: P = D(K1, D(K2, C))
```

For DES, this apparently gives a 112-bit key (`56 × 2`) — but as covered previously, the **meet-in-the-middle attack** cuts that down dramatically, to roughly `2^56` effort. (This was assumed for years but only formally proven in 1992.) The obvious counter is a **third** encryption stage with a third key — 3DES / TDEA (Triple Data Encryption Algorithm), standardized with both a two-key and a three-key version in NIST SP 800-67.

### Two-key Triple DES (EDE)

Proposed by Tuchman: `C = E(K1, D(K2, E(K1, P)))` — an **Encrypt-Decrypt-Encrypt** sequence using only two distinct keys (K1 reused for the first and third stage). The decryption in the middle stage has **no cryptographic significance** — its only purpose is letting 3DES users decrypt data encrypted by plain single-DES users (set `K1 = K2` and the whole thing collapses to single DES). Two-key 3DES was adopted in key-management standards ANSI X9.17 and ISO 8732.

Known attacks against two-key 3DES:

- **Chosen-plaintext (Merkle & Hellman)** — find plaintexts producing a first intermediate value of `A = 0`, then run a meet-in-the-middle attack to find the two keys. Effort: `2^56`, but requires `2^56` chosen plaintext-ciphertext pairs — an unrealistic number for an attacker to obtain from the key holder.
- **Known-plaintext** — an improvement that requires more effort but only known (not chosen) pairs. It's based on the observation that if the intermediate values `A` and `C` are known, the problem reduces to an attack on double DES; the attacker doesn't know `A` directly but can guess a value for it and search for a known `(P, C)` pair producing that guess.

### Three-key Triple DES

Uses three fully independent keys in the same EDE structure — this is the version covered in the previous chapter, giving 168-bit effective strength with no attack close to brute force known.

---

## Why block ciphers need modes of operation

A block cipher encrypts one fixed-length block (`b` bits) at a time. For plaintext longer than `b` bits, the naive approach is to break it into `b`-bit blocks and encrypt each — but doing this carelessly (see ECB, next part) creates security problems when multiple blocks share a key. To handle this, **NIST defined five modes of operation** (SP 800-38A): techniques for enhancing or adapting a block cipher's effect for a given application. All five are designed for use with *any* symmetric block cipher, including 3DES and AES.

| Mode | Description | Typical application |
|---|---|---|
| **ECB** | Each plaintext block encoded independently with the same key. | Secure transmission of single short values (e.g., a key) |
| **CBC** | Input to encryption = XOR of the current plaintext block and the *preceding ciphertext* block. | General-purpose block-oriented transmission; authentication |
| **CFB** | Processes `s` bits at a time; preceding ciphertext feeds the encryption algorithm to produce a pseudorandom output XORed with plaintext. | General-purpose stream-oriented transmission; authentication |
| **OFB** | Like CFB, but the feedback is the *preceding encryption output* itself, and full blocks are used. | Stream-oriented transmission over a noisy channel |
| **CTR** | Each plaintext block XORed with an encrypted counter value, incremented per block. | General-purpose, especially where high speed matters |

### Criteria for evaluating modes (beyond ECB)

- **Overhead** — extra operations required compared to plain ECB.
- **Error recovery** — an error in ciphertext block `i` affects only a few subsequent plaintext blocks before the mode resynchronizes.
- **Error propagation** — a *transmission* bit error in ciphertext block `i` corrupts plaintext block `i` and (depending on the mode) subsequent blocks.
- **Diffusion** — how well plaintext statistics are hidden in the ciphertext (low-entropy/predictable plaintext should not show through).
- **Security** — whether ciphertext blocks leak information about the plaintext.

---

## What to remember for the exam

Two-key 3DES (EDE, K1 reused) is vulnerable to chosen- and known-plaintext attacks that are impractical but weaker than the three-key version's 168-bit strength; its middle decrypt step exists only for single-DES backward compatibility. NIST defines five modes — ECB, CBC, CFB, OFB, CTR — because applying a block cipher block-by-block without care (ECB) leaks patterns; the five criteria to judge a mode are overhead, error recovery, error propagation, diffusion, and security.

---

## Exam check

1. Why does two-key 3DES's middle stage decrypt instead of encrypt?
2. What is the chosen-plaintext attack cost against two-key 3DES, and why is it impractical despite the low operation count?
3. Name the five NIST modes of operation and one typical application for each.
4. List the five criteria used to evaluate a mode of operation.
