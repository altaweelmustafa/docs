---
title: "Chapter 4, Part 4 – Double/Triple DES & Structural Cryptanalysis"
date: 2026-08-23
description: "Why chaining two DES encryptions doesn't double the key strength (meet-in-the-middle), why Triple DES uses three keys in an EDE sequence, and the two major structural attacks on block ciphers: differential and linear cryptanalysis."
tags: [information-security, cryptography, des, triple-des, cryptanalysis, differential-cryptanalysis, linear-cryptanalysis, chapter4]
toc: true
weight: 4
---

## Double DES and the meet-in-the-middle attack

**Double DES** — encrypting twice with two independent 56-bit keys — looks like it should double the effective key length to 112 bits. It doesn't.

The **meet-in-the-middle attack** defeats it: given a known plaintext/ciphertext pair, the attacker encrypts the plaintext under all `2^56` possible first keys, decrypts the ciphertext under all `2^56` possible second keys, and looks for a match in the middle (an intermediate value produced by both directions). This reduces the effective attack cost to roughly `2^56 + 2^56` operations — not much stronger than single DES, and far short of the hoped-for `2^112`.

**Lesson:** simply chaining two encryptions does not multiply security the way it might seem to.

---

## Triple DES with three keys

**Triple DES (3DES)** applies DES three times with three independent keys, in an **Encrypt-Decrypt-Encrypt (EDE)** sequence: encrypt with K1, decrypt with K2, encrypt with K3.

- Effective key length: **168 bits**, with no attack close to brute force known against the full key space.
- The middle *decryption* step exists purely for **backward compatibility**: setting `K1 = K2 = K3` makes 3DES behave exactly like single DES, so 3DES hardware/software can still talk to single-DES systems.
- 3DES was widely used as a transitional standard once single DES was judged too weak, before AES became the default choice.

---

## Beyond brute force: structural cryptanalysis

A brute-force attack tries every key; its cost depends only on key length. **Structural cryptanalysis** instead exploits a cipher's internal design — its S-boxes, round structure, and key schedule — to recover key bits faster than exhaustive search. **Differential** and **linear cryptanalysis** are the two most influential structural attacks on block ciphers, and both directly shaped how DES, AES, and modern ciphers are designed. Both are *statistical* attacks: they don't break a cipher outright, but reduce the effective work factor by finding non-random behavior in the round function.

### Differential cryptanalysis

A **chosen-plaintext attack**, introduced publicly by Biham and Shamir in 1990. Idea: choose pairs of plaintexts with a fixed input difference (XOR), and track how that difference propagates through each round. For a well-behaved (random) function, the output difference would be unpredictable — but real S-boxes leak a bias, where certain output differences occur more often than others. Chaining these biased differences across rounds (a **differential characteristic**) lets an attacker assign probabilities to guesses about round-key bits.

Against **full 16-round DES**, differential cryptanalysis needs roughly **`2^47` chosen plaintexts** — comparable in cost to brute force, so it resists the attack about as well as it resists exhaustive key search. Historical note: IBM's DES design team was reportedly aware of differential cryptanalysis in the 1970s and tuned the S-boxes to resist it, but kept that knowledge confidential; the technique was only rediscovered and published academically in 1990. The lesson: the number of rounds and S-box design in DES were not arbitrary — both were chosen to make differential characteristics decay in probability.

### Linear cryptanalysis

A **known-plaintext attack**, introduced by Matsui in 1993. Idea: find a **linear approximation** — an XOR of specific plaintext bits, ciphertext bits, and key bits — that holds with probability different from one-half. The deviation from 1/2 is the **bias**; the larger the bias, the fewer known plaintext/ciphertext pairs are needed. Given enough pairs, the attacker recovers key bits by counting how often the approximation holds.

Matsui's *Algorithm 1* uses one linear approximation to guess a single key bit; *Algorithm 2* extends this to recover more key bits by attacking the last round directly. Applied to full 16-round DES, linear cryptanalysis needs about **`2^43` known plaintext-ciphertext pairs** — fewer than differential cryptanalysis needs *chosen* pairs, making it the strongest known attack on DES at the time (though still far beyond what's practical against a well-implemented system with proper key management).

### Comparing the two

| | Differential | Linear |
|---|---|---|
| Attack model | Chosen-plaintext | Known-plaintext |
| Exploits | Biased output-difference propagation | Biased linear approximations |
| Cost against DES | ~`2^47` chosen pairs | ~`2^43` known pairs |

Both attacks get exponentially harder as the number of rounds increases, and both directly influenced modern cipher design: **AES's** S-box and round structure were explicitly engineered for *provable* resistance to both, unlike DES, which was hardened empirically.

---

## What to remember for the exam

Double DES only *looks* like it doubles key strength — the meet-in-the-middle attack cuts the effective cost to about `2^57`. Triple DES fixes this with three keys in an EDE sequence (168-bit effective strength; the middle decrypt step is for backward compatibility with single DES). Differential cryptanalysis is chosen-plaintext and tracks biased XOR differences through rounds (~`2^47` against DES); linear cryptanalysis is known-plaintext and tracks a biased linear approximation (~`2^43` against DES). Both confirm that DES's 16 rounds and S-box design were deliberately tuned, and both shaped AES's provably-resistant design.

---

## Exam check

1. Why doesn't Double DES give 112-bit security, and what attack demonstrates this?
2. What does the EDE sequence in Triple DES stand for, and why is the middle step a decryption?
3. Contrast differential and linear cryptanalysis by attack model (chosen vs. known plaintext) and by what each exploits.
4. Roughly how many plaintext pairs does each attack need against full DES, and which is cheaper?
