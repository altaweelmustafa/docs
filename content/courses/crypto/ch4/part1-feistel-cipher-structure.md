---
title: "Chapter 4, Part 1 – Block Ciphers & the Feistel Cipher Structure"
date: 2026-08-23
description: "Block vs. stream ciphers, Feistel's substitution/permutation structure, Shannon's diffusion and confusion, and the design parameters that shape a Feistel cipher."
tags: [information-security, cryptography, block-ciphers, feistel, diffusion, confusion, chapter4]
toc: true
weight: 1
---

## Block ciphers vs. stream ciphers

A **block cipher** treats a block of plaintext as a whole and produces a ciphertext block of equal length — typically 64 or 128 bits. As with a stream cipher, the two users share a symmetric key. The vast majority of network-based symmetric cryptographic applications use block ciphers, so they're the main focus going forward (stream ciphers are covered later, in Chapter 6).

---

## The Feistel cipher

Feistel proposed approximating the ideal block cipher with a **product cipher**: two or more simple ciphers executed in sequence so the combined result is cryptographically stronger than any component alone. Specifically, he proposed alternating:

- **Substitution** — each plaintext element (or group) is uniquely replaced by a corresponding ciphertext element.
- **Permutation** — no elements are added, deleted, or replaced; only their *order* changes.

This is a practical realization of **Claude Shannon's** proposal for a product cipher that alternates **confusion** and **diffusion**. The Feistel structure, dating back to the 1970s (built on Shannon's 1945 proposal), remains the structure behind many significant symmetric ciphers today — including Triple DES (TDEA) and the Camellia cipher.

### Diffusion and confusion

Shannon introduced these two terms to describe how to thwart cryptanalysis based on statistical analysis of the plaintext:

| Concept | What it does |
|---|---|
| **Diffusion** | Dissipates the statistical structure of the plaintext into long-range statistics of the ciphertext — achieved by having each plaintext digit affect many ciphertext digits. |
| **Confusion** | Makes the relationship between the ciphertext statistics and the value of the encryption key as complex as possible, so that even if an attacker finds patterns in the ciphertext, deducing the key from them stays hard. Achieved via a complex substitution algorithm — a simple linear substitution adds little confusion. |

Diffusion and confusion are so effective at capturing the desired attributes of a block cipher that they've become the cornerstone of modern block cipher design.

### Feistel encryption/decryption structure

The plaintext block (length `2w` bits) is split into two halves, `LE0` and `RE0`. Each of `n` rounds takes `LEi-1`, `REi-1`, and a round subkey `Ki` (derived from the overall key `K`, generally different from `K` and from each other round's subkey):

1. A substitution is performed on the left half: apply round function `F` to the right half, then XOR the result with the left half.
2. A permutation follows: the two halves are swapped.

This is a specific form of Shannon's **substitution-permutation network (SPN)**.

**Decryption is essentially the same algorithm run with the subkeys in reverse order** — `Kn` first, then `Kn-1`, and so on down to `K1`. This is a valuable property: no separate decryption algorithm needs to be implemented.

---

## Feistel cipher design parameters

| Parameter | Trade-off |
|---|---|
| **Block size** | Larger → greater security (more diffusion) but slower. 64 bits was traditional; AES uses 128. |
| **Key size** | Larger → greater security (more resistance to brute force, more confusion) but potentially slower. 64 bits or less is now inadequate; 128 bits is common. |
| **Number of rounds** | A single round is inadequate; multiple rounds increase security. 16 rounds is typical. |
| **Subkey generation algorithm** | Greater complexity → greater difficulty of cryptanalysis. |
| **Round function F** | Greater complexity → greater resistance to cryptanalysis. |

Two further considerations shape practical designs:

- **Fast software encryption/decryption** — speed matters when the cipher is embedded in an application without a hardware implementation.
- **Ease of analysis** — an algorithm that can be concisely and clearly explained is easier to analyze for vulnerabilities, giving higher assurance of its strength. (DES, notably, does *not* have an easily analyzed functionality.)

---

## What to remember for the exam

Feistel = alternating substitution and permutation, a practical form of Shannon's confusion/diffusion product cipher. Diffusion spreads plaintext statistics across the ciphertext; confusion complicates the ciphertext-to-key relationship. Decryption in a Feistel cipher reuses the encryption algorithm with subkeys reversed. Five design parameters: block size, key size, number of rounds, subkey generation, and round function F.

---

## Exam check

1. Define substitution and permutation as Feistel uses the terms.
2. What is the difference between diffusion and confusion, and which Shannon concepts do they realize?
3. Why does a Feistel cipher not need a separate decryption algorithm?
4. List the five parameters that define a specific Feistel cipher's design.
