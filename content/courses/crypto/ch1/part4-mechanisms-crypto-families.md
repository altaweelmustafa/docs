---
title: "Chapter 1, Part 4 – Security Mechanisms & Cryptographic Algorithm Families"
date: 2026-08-09
description: "The eight X.800 security mechanisms, plus the keyless / single-key / two-key classification of cryptographic algorithms."
tags: [information-security, security-mechanisms, cryptography, chapter1]
toc: true
weight: 4
---

## Security mechanisms (X.800)

These are the building blocks that implement the services from Part 3.

| Mechanism | What it does |
|---|---|
| **Cryptographic algorithms** | Reversible (encryption — data can be decrypted back) or irreversible (hash functions, MACs — used for signatures and authentication). |
| **Data integrity** | A family of mechanisms that check a data unit or stream hasn't been tampered with. |
| **Digital signature** | Data (or a transformation of data) appended to a unit so a recipient can prove its source and integrity, and it resists forgery. |
| **Authentication exchange** | Confirms an entity's identity through an information exchange. |
| **Traffic padding** | Inserts filler bits into gaps in a data stream to frustrate traffic analysis. |
| **Routing control** | Chooses secure physical/logical routes for sensitive data, and allows re-routing if a breach is suspected. |
| **Notarization** | Uses a trusted third party to guarantee properties of a data exchange (e.g. that it occurred, or when). |
| **Access control** | Mechanisms that enforce access rights to resources. |

---

## Cryptographic algorithms: three families

Cryptographic algorithms (Figure 1.4) split into three categories based on how many keys they use:

### 1. Keyless algorithms

No key is involved — deterministic functions with properties useful for cryptography.

- **Cryptographic hash function** — turns an arbitrary amount of text into a small, fixed-length digest/hash value. A *cryptographic* hash function has extra properties that make it safe to use inside a MAC or digital signature.
- **Pseudorandom number generator (PRNG)** — deterministic, but produces a sequence that *looks* truly random.

### 2. Single-key (symmetric) algorithms

Both parties share one **secret key**. Forms:

- **Block cipher** — operates on fixed-size blocks of data; in most modes of operation, the transformation of a block depends on the key *and* on previous blocks.
- **Stream cipher** — operates on a continuous sequence of bits, transforming them one at a time (typically via XOR) as a function of the key.
- **Message Authentication Code (MAC)** — not encryption at all: a data element derived from a message plus a secret key, used to *verify integrity*. The recipient recomputes the MAC and compares it — a mismatch means the message was altered.

### 3. Two-key (asymmetric) algorithms

Uses a **private key** (known to one party) and a related **public key** (widely distributed). Applications:

- **Digital signature algorithm** — the signer uses their private key to sign; anyone with the public key can verify origin and integrity.
- **Key exchange** — securely distributing a symmetric key to two or more parties.
- **User authentication** — proving a user (or a service) is genuinely who it claims to be.

> Exam sentence: keyless → no key; single-key → one shared secret key (symmetric); two-key → a private/public key pair (asymmetric). Block/stream ciphers and MACs are single-key; digital signatures, key exchange, and asymmetric encryption are two-key.

---

## What to remember for the exam

Mechanisms are the *tools* (cryptographic algorithms, digital signatures, access control, etc.); cryptographic algorithms themselves split into keyless, single-key, and two-key families based on how many keys are used and by whom.

---

## Exam check

1. Name the eight X.800 security mechanisms.
2. What distinguishes a keyless algorithm from a single-key algorithm?
3. Why is a MAC not the same thing as encryption, even though it uses a secret key?
