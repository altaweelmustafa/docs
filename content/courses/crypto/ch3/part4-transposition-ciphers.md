---
title: "Chapter 3, Part 4 – Transposition Techniques"
date: 2026-08-09
description: "Rail fence and row (columnar) transposition ciphers, which permute letters instead of substituting them."
tags: [information-security, cryptography, transposition-ciphers, chapter3]
toc: true
weight: 4
---

## Transposition vs. substitution

Every technique so far substituted one symbol for another. A **transposition cipher** instead **permutes** (rearranges) the plaintext letters — no letter is replaced, just moved.

---

## Rail fence cipher

The simplest transposition technique. Write the plaintext diagonally across a set number of "rails," then read off row by row.

**Example** — `"meet me after the toga party"`, depth 2:

```
m e m a t r h t g p r y
 e t e f e t e o a a t
```

Reading row by row gives the ciphertext:

```
MEMATRHTGPRYETEFETEOAAT
```

---

## Row (columnar) transposition cipher

A more complex scheme: write the plaintext into a rectangle **row by row**, then read it off **column by column** — but visit the columns in an order given by a key. That column order *is* the key.

**Example** — key (column order): `4 3 1 2 5 6 7`

```
Plaintext, written row by row:
a t t a c k p
o s t p o n e
d u n t i l t
w o a m x y z
```

Encrypt by reading columns in the order the key specifies: start with the column labeled `1` (column 3), then column labeled `2` (column 4), then column 2, then column 1, then columns 5, 6, 7.

**Ciphertext:**

```
TTNAAPTMTSUOAODWCOIXKNLYPETZ
```

### Cryptanalysis

A pure transposition cipher keeps the **same letter frequencies** as the plaintext — that's the giveaway that identifies it as transposition rather than substitution. Breaking it involves laying the ciphertext into a matrix and experimenting with column orders, often guided by digram/trigram frequency tables.

**Strengthening it:** apply transposition in **multiple stages** — the result is a more complex permutation that's much harder to reconstruct than a single pass.

> Exam sentence: substitution ciphers change letter *identity* but preserve *position*; transposition ciphers change letter *position* but preserve *identity* (and therefore preserve the plaintext's letter-frequency profile).

---

## What to remember for the exam

Rail fence: write diagonally across N rails, read by row. Row transposition: write in a grid row by row, read by column in key-defined order. Transposition ciphers are detectable because letter frequency matches the plaintext language; multiple rounds of transposition increase security.

---

## Exam check

1. Encrypt `"COMPUTER SCIENCE"` with a rail fence cipher of depth 3.
2. Why does a pure transposition cipher preserve letter frequency, and why is that a weakness?
3. How does applying transposition twice in a row improve security over a single pass?
