---
title: "Chapter 5, Part 4 – Counter (CTR) Mode & Comparing All Five Modes"
date: 2026-08-23
description: "CTR mode's parallel-friendly counter-based construction, its six practical advantages, and how feedback structure separates ECB/CTR from the three chaining modes."
tags: [information-security, cryptography, modes-of-operation, ctr, chapter5]
toc: true
weight: 4
---

## Counter (CTR) mode

Proposed back in 1979, CTR mode uses a **counter** the same size as the plaintext block. The only hard requirement (SP 800-38A): the counter value must be **different for every plaintext block encrypted**. Typically it starts at some value and increments by 1 per block (mod `2^b`).

**Encryption:** encrypt the counter value, then XOR the result with the plaintext block to get the ciphertext block — **no chaining**. **Decryption:** encrypt the same sequence of counter values, XOR each with the corresponding ciphertext block. The initial counter value must therefore be available to the receiver.

As with OFB, the initial counter value must be a **nonce** — unique across all messages encrypted under a given key. Reusing a counter value is dangerous: if an attacker knows any plaintext block encrypted under a given counter value, they can recover the encryption function's output for that value, and then decrypt *any other* ciphertext block that reused the same counter value. A simple way to guarantee uniqueness: keep incrementing the counter across messages, rather than resetting it to a fixed start each time.

### Advantages of CTR mode

1. **Hardware efficiency** — unlike the three chaining modes (CBC, CFB, OFB), CTR can encrypt/decrypt multiple blocks **in parallel**, since no block depends on a previous one; throughput is limited only by available parallelism.
2. **Software efficiency** — the same parallelism benefits pipelining, multiple instruction dispatch, and SIMD instructions on modern processors.
3. **Preprocessing** — since the encryption step doesn't depend on the plaintext/ciphertext at all, the encrypted-counter values can be computed ahead of time; once data arrives, only a fast XOR remains.
4. **Random access** — the `i`th block can be decrypted directly, without decrypting the blocks before it (impossible with chaining modes) — useful when only part of a stored ciphertext needs decrypting.
5. **Provable security** — CTR can be shown to be at least as secure as the other modes.
6. **Simplicity** — CTR needs only the **encryption** algorithm implemented, not decryption (and not the decryption key schedule) — significant when encryption and decryption differ substantially, as in AES.

---

## Comparing all five modes

With the sole exception of ECB, every NIST-approved mode involves **feedback** of some kind — the crucial difference is *what* feeds back and *how independent* it is of the plaintext:

| Mode | Feedback | Chaining? | Parallelizable encryption? | Output independent of plaintext? |
|---|---|---|---|---|
| **ECB** | None | No | Yes | No (deterministic per block) |
| **CBC** | Preceding ciphertext block, XORed before encryption | Yes | No | No |
| **CFB** | Preceding ciphertext, via shift register | Yes | No | No |
| **OFB** | Preceding encryption *output* | Yes | No | Yes |
| **CTR** | None (independent counters) | No | **Yes** | Yes |

**OFB** and **CTR** both produce output independent of plaintext and ciphertext, making them natural candidates for use as stream ciphers that XOR one full block at a time against the plaintext.

---

## What to remember for the exam

CTR mode encrypts a per-block counter and XORs it with the plaintext — no chaining, so blocks can be processed in parallel, preprocessed ahead of time, accessed randomly, and only the encryption algorithm (not decryption) is ever needed. Like OFB, its counter/IV must never repeat under the same key. Across all five modes, only ECB lacks feedback entirely; CBC and CFB chain on ciphertext, while OFB and CTR generate a plaintext-independent stream — which is what makes the latter two usable as general stream ciphers.

---

## Exam check

1. What must be true of every counter value used in CTR mode, and why does reusing one break confidentiality?
2. List CTR mode's six advantages over the chaining modes.
3. Which two modes produce output that is completely independent of the plaintext, and why does that matter?
4. Why does CTR mode not require implementing a decryption algorithm at all?
