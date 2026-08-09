---
title: "Chapter 3, Part 2 – Substitution Techniques: Caesar, Monoalphabetic, Playfair & Hill"
date: 2026-08-09
description: "Classical substitution ciphers, letter-frequency cryptanalysis, and the multi-letter Playfair and Hill ciphers."
tags: [information-security, cryptography, substitution-ciphers, caesar-cipher, playfair-cipher, hill-cipher, chapter3]
toc: true
weight: 2
---

## Substitution technique

Letters of the plaintext are replaced by other letters, numbers, or symbols (or, at the bit level, plaintext bit patterns are replaced by ciphertext bit patterns).

---

## Caesar cipher

The earliest and simplest substitution cipher, attributed to Julius Caesar: shift every letter three places down the alphabet, wrapping `Z → A`.

**Generalized to any shift `k`:**

```
c = E(k, p) = (p + k) mod 26        (encryption)
p = D(k, c) = (c − k) mod 26        (decryption)
```

with letters numbered `a=0, b=1, ..., z=25` and `k` in the range 1–25.

**Weakness:** only 25 possible keys — trivially brute-forceable.

---

## Monoalphabetic cipher

Instead of a fixed shift, allow the "cipher alphabet" to be **any permutation** of the 26 letters. That gives `26! ≈ 4×10²⁶` possible keys — far too many to brute-force (10 orders of magnitude more than DES's key space!).

**But it's still weak**, because a monoalphabetic substitution preserves the **frequency statistics** of the underlying language.

### Frequency analysis

English letter frequencies are far from uniform (e.g. `e` and `t` are common; `z`, `q` are rare). A cryptanalyst can:

1. Tally letter frequencies in the ciphertext and match them against known English frequencies.
2. Use **digrams** (2-letter combinations — most common is `th`) and **trigrams** (3-letter — most common is `the`) to pin down specific letters once the high-frequency singles are guessed.
3. Iterate: partial guesses reveal word fragments, which confirm or rule out other guesses.

Given enough ciphertext, this is usually enough to fully break a monoalphabetic cipher — this is why the countermeasure of **homophones** (assigning multiple cipher symbols to one plaintext letter, proportional to its frequency) was introduced. Even homophones don't fully solve the problem, though, because multi-letter patterns (digram frequencies) still leak through.

> Exam sentence: a huge key space (26! for monoalphabetic) does **not** guarantee security — frequency analysis breaks monoalphabetic ciphers regardless of key-space size, because each plaintext letter always maps to the same ciphertext letter.

---

## Playfair cipher

Invented by Sir Charles Wheatstone (1854); used as a field cipher by the British Army in WWI and the US Army in WWII. Encrypts **digrams** (letter pairs) as single units instead of individual letters.

### Building the key matrix

Fill a 5×5 grid with the letters of a keyword (duplicates removed), left-to-right/top-to-bottom, then fill the rest with remaining letters alphabetically. `I` and `J` share one cell.

**Example — keyword `MONARCHY`:**

| M | O | N | A | R |
|---|---|---|---|---|
| C | H | Y | B | D |
| E | F | G | I/J | K |
| L | P | Q | S | T |
| U | V | W | X | Z |

### Encryption rules (for each plaintext digram)

1. **Same-pair repeat**: if a digram would repeat the same letter, insert a filler (e.g. `x`) between them. `balloon` → `ba lx lo on`.
2. **Same row**: replace each letter with the one to its right (wrapping around). `ar → RM`.
3. **Same column**: replace each letter with the one below it (wrapping around). `mu → CM`.
4. **Otherwise (rectangle rule)**: each letter is replaced by the letter in its own row but the other letter's column. `hs → BP`, `ea → IM`.

**Why it's stronger than monoalphabetic:** 26×26 = 676 possible digrams vs. 26 letters, so frequency analysis is much harder. **Why it's still breakable:** a few hundred letters of ciphertext are typically enough, since digram structure still leaks information.

---

## Hill cipher

Developed by Lester Hill (1929). Uses **matrix arithmetic mod 26**: an `m×m` key matrix encrypts `m` plaintext letters at once into `m` ciphertext letters, via linear equations over `Z26`.

- **Strength**: completely hides single-letter frequency (and with a 3×3+ matrix, two-letter frequency too).
- **Weakness**: strong against ciphertext-only attacks, but easily broken with a **known-plaintext** attack (enough plaintext/ciphertext pairs let you solve for the key matrix directly with linear algebra).

### Worked example (2×2 key)

Letters as numbers (`A=0 ... Z=25`). Key matrix `K = [[3,3],[2,5]]` — invertible mod 26 since `gcd(det K, 26) = gcd(9, 26) = 1`.

Encrypt `"HI"` → plaintext vector `(7, 8)`:

```
c1 = (3×7 + 3×8) mod 26 = 45 mod 26 = 19 → 'T'
c2 = (2×7 + 5×8) mod 26 = 54 mod 26 =  2 → 'C'
```

So `HI → TC`. Decryption uses `K⁻¹ mod 26` applied to the ciphertext vector to recover the plaintext.

---

## What to remember for the exam

Caesar: single shift, 25 keys, trivial. Monoalphabetic: huge key space but broken by frequency analysis. Playfair: encrypts digrams via a 5×5 keyword matrix with 4 rules (repeat/row/column/rectangle) — much stronger, still breakable. Hill: matrix multiplication mod 26, hides multi-letter frequency, strong against ciphertext-only but weak against known-plaintext.

---

## Exam check

1. Encrypt `"HELP"` with a Caesar cipher, `k = 5`.
2. Using the `MONARCHY` Playfair matrix, encrypt the digram `"HS"`, showing which rule applies.
3. Why is the Hill cipher vulnerable to a known-plaintext attack even though it hides letter frequencies well?
