---
title: "Chapter 4, Part 2 – The Data Encryption Standard (DES)"
date: 2026-08-23
description: "DES history and structure, the initial/final permutations wrapped around 16 Feistel rounds, timing-attack resistance, and why the number of rounds is chosen the way it is."
tags: [information-security, cryptography, des, block-ciphers, chapter4]
toc: true
weight: 2
---

## History

**DES** was issued in 1977 by the National Bureau of Standards (now NIST) as **FIPS PUB 46**, and was the most widely used symmetric cipher until AES arrived in 2001. The algorithm itself is the **Data Encryption Algorithm (DEA)**: it encrypts 64-bit blocks using a 56-bit key, transforming 64-bit input into 64-bit output through a series of steps; the same steps with the same key reverse the encryption.

DES dominated for years, especially in finance. In 1999, NIST's FIPS PUB 46-3 restricted DES to legacy systems and recommended **Triple DES** instead (covered in Part 4). Because DES and Triple DES share the same underlying algorithm, understanding single DES remains essential.

---

## Structure of DES encryption

Two inputs: a 64-bit plaintext block and a 56-bit key. Processing has three phases:

1. **Initial permutation (IP)** — rearranges the 64 plaintext bits to produce the permuted input.
2. **16 rounds** of the same function, each combining permutation and substitution (the Feistel structure from Part 1).
3. The left and right halves of the round-16 output are **swapped** to produce the preoutput, which then passes through **IP⁻¹** (the inverse of the initial permutation) to produce the 64-bit ciphertext.

Except for the initial and final permutations, DES has the exact structure of a Feistel cipher. On the key side: the 56-bit key first passes through a permutation, and then for each of the 16 rounds a **subkey `Ki`** is produced via a left circular shift plus a (per-round-identical) permutation function — different subkeys result because the shifts accumulate differently each round.

As with any Feistel cipher, decryption uses the same algorithm with the subkeys applied in reverse order, and the initial/final permutations reversed.

---

## Strength of DES: timing attacks

A **timing attack** recovers information about the key or plaintext by observing how long a given implementation takes to decrypt various ciphertexts — exploiting the fact that encryption/decryption often takes slightly different time on different inputs. Research has extracted the Hamming weight (number of 1-bits) of the secret key this way — far from the full key, but a notable first step. So far, it appears **unlikely** this technique will succeed against DES or stronger ciphers like Triple DES and AES.

---

## Design principle: number of rounds

The greater the number of rounds, the harder cryptanalysis becomes, even against a relatively weak round function F. The general criterion: choose the number of rounds so that known cryptanalytic attacks require **more** effort than a brute-force key search.

This criterion drove DES's design directly. For 16-round DES, differential cryptanalysis (Part 5) requires about `2^55.1` operations — slightly *less* efficient than brute force's `2^55`. **If DES had 15 or fewer rounds, differential cryptanalysis would beat brute force.** This is why this criterion is attractive: in the absence of a cryptanalytic breakthrough, an algorithm meeting it can be judged on key length alone.

---

## What to remember for the exam

DES: FIPS PUB 46 (1977), 64-bit blocks, 56-bit key, 16 Feistel rounds bracketed by IP and IP⁻¹. Timing attacks are a theoretical concern but not currently practical against DES. The number of rounds (16) is chosen specifically so that the best known cryptanalytic attack (differential cryptanalysis) costs slightly *more* than brute force — one fewer round would flip that.

---

## Exam check

1. What are the block size and key size of DES?
2. Describe the three processing phases of DES encryption.
3. What does a timing attack exploit, and how effective is it against DES?
4. Why is 16 rounds the right number for DES, and not, say, 15?
