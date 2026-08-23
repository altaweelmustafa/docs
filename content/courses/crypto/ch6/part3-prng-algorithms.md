---
title: "Chapter 6, Part 3 – PRNG Algorithm Design: LCG, Blum Blum Shub & Block-Cipher-Based PRNGs"
date: 2026-08-23
description: "The two families of PRNG algorithm, the linear congruential generator, the provably-secure Blum Blum Shub generator, and how CTR/OFB modes turn a block cipher into a PRNG."
tags: [information-security, cryptography, prng, lcg, blum-blum-shub, ctr-drbg, chapter6]
toc: true
weight: 3
---

## Two families of PRNG algorithm

- **Purpose-built algorithms** — designed specifically and solely to generate pseudorandom bit streams (some general-purpose, some built for a specific stream cipher).
- **Algorithms based on existing cryptographic primitives** — since cryptographic algorithms are required to randomize their input (regular patterns in ciphertext would aid cryptanalysis), they double as PRNG cores. SP 800-90A recommends three such categories: **symmetric block ciphers**, **asymmetric ciphers**, and **hash functions/MACs**.

---

## Linear Congruential Generator (LCG)

A widely used purpose-built technique, first proposed by Lehmer. Parameterized by four values:

| Parameter | Meaning | Constraint |
|---|---|---|
| `m` | modulus | `m > 0` |
| `a` | multiplier | `0 < a < m` |
| `c` | increment | `0 ≤ c < m` |
| `X0` | starting value (seed) | `0 ≤ X0 < m` |

The sequence is generated iteratively:

```
X(n+1) = (a·Xn + c) mod m
```

This produces integers in the range `0 ≤ Xn < m`. The choice of `a`, `c`, and `m` is critical to generator quality — `m` should be large (a value near `2^31` is typical) so there's potential for a long run of distinct values before the sequence repeats.

---

## Blum Blum Shub (BBS) generator

Arguably the purpose-built algorithm with the **strongest public proof of cryptographic strength**. BBS is a **cryptographically secure pseudorandom bit generator (CSPRBG)** — meaning it passes the **next-bit test**: there is no polynomial-time algorithm that, given the first `k` output bits, can predict bit `k+1` with probability significantly greater than 1/2. In other words, for all practical purposes, the sequence is unpredictable.

**BBS's security rests on the difficulty of factoring `n`** — i.e., given a modulus `n = p·q`, finding its two prime factors is computationally infeasible for large enough `n`. This ties BBS's strength directly to the same hard problem that underlies RSA (covered later in the course).

---

## Building a PRNG from a block cipher

Two block-cipher-based constructions have gained widespread acceptance, both recommended in NIST SP 800-90A / ANSI X9.82 / RFC 4086:

- **CTR mode**
- **OFB mode**

In both, the seed consists of two parts: an encryption **key** and a value **`V`** that gets updated after each generated block (e.g., for AES-128, a 128-bit key + a 128-bit V). The difference is in how `V` updates:

- **CTR-based:** `V` is simply **incremented by 1** after each encryption.
- **OFB-based:** `V` is updated to equal the **preceding PRNG output block**.

In both cases, pseudorandom bits come out one full block at a time (128 bits at a time, for AES).

### CTR_DRBG

A concrete NIST construction with two phases: an **initialize/update** function that establishes `Key` and `V`, and a **generate** function that iterates the encryption operation to produce as many pseudorandom bits as needed — reusing the same key each iteration while incrementing `V` by 1 per iteration. To bound how much output any single key/seed pair produces (limiting exposure), CTR_DRBG uses a `reseed_interval` parameter.

---

## What to remember for the exam

LCG: `X(n+1) = (aXn + c) mod m` — simple and fast, but not cryptographically strong on its own; good `a`, `c`, `m` selection matters. BBS is provably strong (passes the next-bit test), with security resting on the difficulty of factoring `n` — the same hardness assumption RSA relies on. Block ciphers can drive a PRNG via CTR mode (increment `V`) or OFB mode (feed back the previous output as `V`); CTR_DRBG is NIST's standard construction, using a reseed interval to cap output per seed.

---

## Exam check

1. Write the LCG recurrence and name its four parameters.
2. What does it mean for BBS to pass the "next-bit test"?
3. What hard mathematical problem is BBS's security based on?
4. How do the CTR-based and OFB-based block-cipher PRNG constructions differ in updating `V`?
