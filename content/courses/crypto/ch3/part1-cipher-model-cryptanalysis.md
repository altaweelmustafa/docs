---
title: "Chapter 3, Part 1 – Basic Concepts & the Symmetric Cipher Model"
date: 2026-08-09
description: "Core terminology, the five ingredients of a symmetric cipher, the three dimensions cryptosystems are characterized along, and the five types of cryptanalytic attack."
tags: [information-security, cryptography, symmetric-encryption, cryptanalysis, chapter3]
toc: true
weight: 1
---

## Core terminology

| Term | Meaning |
|---|---|
| **Plaintext** | The original, intelligible message. |
| **Ciphertext** | The coded (unintelligible) message. |
| **Encryption / enciphering** | Converting plaintext → ciphertext. |
| **Decryption / deciphering** | Restoring plaintext from ciphertext. |
| **Cryptography** | The study of schemes used for encryption. |
| **Cryptographic system / cipher** | A particular such scheme. |
| **Cryptanalysis** | Techniques for deciphering a message *without* knowing the enciphering details — "breaking the code." |
| **Cryptology** | Cryptography + cryptanalysis together. |

---

## The symmetric cipher model

A symmetric encryption scheme has five ingredients (Figure 3.1):

1. **Plaintext** — the original data fed into the algorithm.
2. **Encryption algorithm** — performs substitutions/transformations on the plaintext.
3. **Secret key** — independent of both plaintext and algorithm; the exact transformation depends on this value.
4. **Ciphertext** — the scrambled output; different keys on the same plaintext give different ciphertexts.
5. **Decryption algorithm** — essentially the encryption algorithm run in reverse, taking ciphertext + key back to plaintext.

**Two requirements** for secure use of symmetric encryption:

1. A **strong algorithm** — an opponent who knows the algorithm and has ciphertexts (even ciphertext/plaintext pairs) shouldn't be able to recover the key or decrypt other ciphertexts.
2. Sender and receiver must **share the secret key securely** and keep it protected.

Crucially, the algorithm itself does **not** need to be secret — only the key does. This is what allows cheap, mass-produced hardware/software implementations of standard algorithms.

---

## Three dimensions that characterize a cryptosystem

1. **Type of operation**: substitution (elements are replaced by other elements) or transposition (elements are rearranged).
2. **Number of keys**: symmetric/single-key/secret-key/conventional (one shared key) vs. asymmetric/two-key/public-key (sender and receiver use different keys).
3. **How plaintext is processed**: block cipher (fixed-size blocks at a time) vs. stream cipher (continuous element-by-element processing).

---

## Cryptanalysis vs. brute-force attack

| | Cryptanalysis | Brute-force |
|---|---|---|
| Approach | Exploits weaknesses in the algorithm's structure, plus knowledge of the plaintext's likely characteristics | Tries every possible key until an intelligible plaintext appears |
| Cost | Depends on algorithm weaknesses | On average, half the key space must be tried |

If either succeeds in recovering the key, **all** past and future messages under that key are compromised.

### Types of cryptanalytic attack (by what the analyst knows)

| Attack type | Known to the cryptanalyst |
|---|---|
| **Ciphertext only** | Algorithm + ciphertext. (Hardest attack — least information.) |
| **Known plaintext** | Algorithm + ciphertext + one or more plaintext/ciphertext pairs. |
| **Chosen plaintext** | Algorithm + ciphertext + a plaintext of the analyst's choosing, along with its ciphertext. |
| **Chosen ciphertext** | Algorithm + ciphertext + a ciphertext of the analyst's choosing, along with its decrypted plaintext. |
| **Chosen text** | A combination of chosen plaintext and chosen ciphertext. |

Ciphertext-only is easiest to defend against (least analyst information); as the analyst gains more chosen material, attacks become progressively more powerful.

> Exam sentence: the key never needs to stay secret in terms of the *algorithm* — Kerckhoffs's principle (implicit here) says security should rest entirely on the secrecy of the key, not the algorithm.

---

## What to remember for the exam

Five ingredients of a symmetric scheme: plaintext, algorithm, key, ciphertext, decryption algorithm. Three characterizing dimensions: operation type, key count, processing style. Five attack types ranked by how much the analyst knows, from ciphertext-only (weakest) to chosen-text (strongest).

---

## Exam check

1. List and explain the five ingredients of a symmetric encryption scheme.
2. Why doesn't the encryption *algorithm* need to be kept secret?
3. Rank the five cryptanalytic attack types from least to most information available to the attacker.
