---
title: "Chapter 5, Part 2 – Electronic Codebook (ECB) & Cipher Block Chaining (CBC)"
date: 2026-08-23
description: "ECB's codebook property and why it's weak for long messages, and how CBC's chaining via XOR and an initialization vector fixes it."
tags: [information-security, cryptography, modes-of-operation, ecb, cbc, chapter5]
toc: true
weight: 2
---

## Electronic Codebook (ECB) mode

The simplest mode: each plaintext block is encrypted independently with the same key.

```
ECB:  Cj = E(K, Pj)   Pj = D(K, Cj)     for j = 1, ..., N
```

It's called "codebook" because, for a given key, there's a unique ciphertext for every possible `b`-bit plaintext block — as if a giant lookup table mapped every plaintext pattern to its ciphertext. A message longer than `b` bits is simply broken into `b`-bit blocks (padding the last one if needed), and each is encrypted/decrypted independently with the same key.

**The defining weakness:** if the same `b`-bit plaintext block appears more than once in a message, it **always produces the same ciphertext block**. For long or structured messages, this can leak enough regularity for a cryptanalyst to exploit — known plaintext/ciphertext pairs from predictable header fields, or repeated elements at multiples of `b` bits that aid analysis or enable block substitution/rearrangement attacks. **ECB should only be used for messages shorter than one block** (e.g., encrypting a single secret key) — for anything longer, its practical value is minimal.

---

## Cipher Block Chaining (CBC) mode

CBC fixes ECB's core weakness: the same plaintext block, if repeated, should produce **different** ciphertext blocks.

**Mechanism:** the input to the encryption algorithm is the **XOR of the current plaintext block and the preceding ciphertext block**; the same key is used throughout. This chains the processing of the whole plaintext sequence together, so the input to encryption for any given block has no fixed relationship to that block's plaintext alone — repeating `b`-bit patterns are no longer exposed. As with ECB, the last block is padded if partial.

**Decryption:** pass each ciphertext block through the decryption algorithm, then XOR the result with the *preceding* ciphertext block to recover the plaintext block.

### The initialization vector (IV)

To produce the *first* block of ciphertext (which has no preceding ciphertext block to XOR with), an **IV** — a data block the same size as the cipher block — is XORed with the first plaintext block instead. On decryption, the IV is XORed with the decryption output of the first block to recover it.

Requirements on the IV:

- Known to both sender and receiver, but **unpredictable** by a third party — it must not be possible to predict the IV in advance of it being generated for a given plaintext. (If an opponent can fool the receiver into using a different IV, they can invert selected bits of the recovered first plaintext block.)
- For maximum security, it should also be protected against unauthorized changes (e.g., by sending it under ECB encryption).
- SP800-38A recommends generating it either by encrypting a **nonce** (a counter, timestamp, or message number — unique per encryption operation) under the same key used for the message, or by drawing it from a random number generator.

Because of its chaining mechanism, CBC is the appropriate mode for messages longer than `b` bits, and — beyond confidentiality — it can also be used for authentication (covered later in the course).

---

## What to remember for the exam

ECB encrypts every block independently under the same key — simple, but identical plaintext blocks always yield identical ciphertext blocks, which leaks structure; use it only for single-block messages. CBC XORs each plaintext block with the *previous ciphertext block* before encrypting, chaining the whole message together and hiding repeated patterns; it needs an IV (unpredictable, ideally protected) to bootstrap the first block.

---

## Exam check

1. Why is ECB unsuitable for messages longer than a single block?
2. Write the ECB encryption equation for block `j`.
3. What does CBC XOR together before encrypting a block, and how does that differ for the very first block?
4. What two properties must a CBC initialization vector have?
