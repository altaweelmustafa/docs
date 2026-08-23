---
title: "Chapter 4, Part 3 – Simplified DES (S-DES): A Worked Example"
date: 2026-08-23
description: "S-DES key generation and encryption structure, its permutation/S-box tables, and a full hand-traced numeric example — the same structure as DES at a scale you can compute on paper."
tags: [information-security, cryptography, s-des, des, feistel, chapter4]
toc: true
weight: 3
---

## Why S-DES

**Simplified DES (S-DES)** is a teaching cipher developed by Edward Schaefer to illustrate DES's structure with hand-computable numbers. It operates on **8-bit plaintext blocks with a 10-bit key**, producing 8-bit ciphertext, using the same design principles as full DES: an initial permutation, Feistel-like rounds of substitution/permutation, and a final permutation. Working through S-DES by hand makes the much larger DES structure (64-bit blocks, 56-bit key, 16 rounds) easier to follow conceptually.

| | S-DES | DES |
|---|---|---|
| Block size | 8 bits | 64 bits |
| Key size | 10 bits | 56 bits |
| Rounds | 2 | 16 |
| IP size | 8 bits | 64 bits |
| S-Boxes | 2 | 8 |
| Round keys | 2 | 16 |
| Round key size | 8 bits | 48 bits |

---

## Key generation

Starting from a single 10-bit key `K`:

1. Permute the 10 bits with **P10**, then split into two 5-bit halves.
2. Apply a left circular shift (**LS-1**) to each half, recombine, and permute with **P8** (8 of the 10 bits selected and reordered) to produce the first subkey `K1`.
3. Apply a second, larger left circular shift (**LS-2**) to the LS-1 result, and a second **P8** to produce `K2`.

`K1` is used in round 1 of encryption, `K2` in round 2.

### Permutation and operation tables

| P10 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|---|
| Output | 3 | 5 | 2 | 7 | 4 | 10 | 1 | 9 | 8 | 6 |

| P8 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|---|---|---|---|---|---|---|---|---|
| Output | 6 | 3 | 7 | 4 | 8 | 5 | 10 | 9 |

| P4 | 1 | 2 | 3 | 4 |
|---|---|---|---|---|
| Output | 2 | 4 | 3 | 1 |

| IP | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|---|---|---|---|---|---|---|---|---|
| Output | 2 | 6 | 3 | 1 | 4 | 8 | 5 | 7 |

| IP⁻¹ | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|---|---|---|---|---|---|---|---|---|
| Output | 4 | 1 | 3 | 5 | 7 | 2 | 8 | 6 |

| EP (Expansion/Permutation) | 1 | 2 | 3 | 4 | (2) | (3) | (4) | (1) |
|---|---|---|---|---|---|---|---|---|
| Output | 4 | 1 | 2 | 3 | 2 | 3 | 4 | 1 |

Other building blocks: **CLS-1 / CLS-2** (circular left shift by 1 / 2 positions), **SW** (swap the two halves), **fK** (the round function using round key K), **F** (the internal substitution function inside each round), **XOR (⊕)** (bitwise exclusive-or: `0⊕0=0`, `0⊕1=1`, `1⊕0=1`, `1⊕1=0`).

---

## Encryption structure

```
8-bit plaintext
  → Initial Permutation (IP)
  → Round 1: function fK using K1
  → Switch (swap halves)
  → Round 2: function fK using K2
  → IP⁻¹
  → 8-bit ciphertext
```

Like full DES, S-DES alternates substitution/permutation rounds driven by subkeys derived from the master key. **Decryption simply reverses the subkey order** (K2 then K1) — the same property Feistel ciphers give us in general.

### S-Boxes

Each S-box is treated as a 4×4 matrix. Given a 4-bit input `bit1 bit2 bit3 bit4`: **bit1 bit4** selects the row (0–3 in decimal), **bit2 bit3** selects the column (0–3), and the selected matrix entry is the 2-bit output. S0 processes the left half of the post-XOR value; S1 processes the right half.

---

## Worked example

**Encrypt plaintext `01110010`** (= `0x72` = `'r'` in ASCII) **with key `1010000010`.**

### Round-key generation

| Step | Result |
|---|---|
| `K` (10-bit) | `1010000010` |
| P10(K) | `10000 01100` |
| LS-1 (both halves) | `00001 11000` |
| P8 → **K1** | `10100100` (= `A4` hex) |
| LS-2 (on LS-1 result, both halves) | `00100 00011` |
| P8 → **K2** | `01000011` (= `43` hex) |

### Round 1

| Step | Value |
|---|---|
| IP(plaintext) | `10101001` |
| Right half | `1001` |
| EP(right half) | `11000011` |
| XOR with K1 (`10100100`) | `01100111` |
| S0 on left nibble `0110`: row `00`=0, col `11`=3 → output `10` | |
| S1 on right nibble `0111`: row `01`=1, col `11`=3 → output `11` | |
| S0‖S1 | `1011` |
| P4(S0‖S1) | `0111` |
| XOR with left half (`1010`) | `1101` |
| Result = XOR-output ‖ right half | `11011001` (end of round 1) |
| SW (swap halves) | `10011101` → feeds into round 2 |

### Round 2

| Step | Value |
|---|---|
| Right half of SW result | `1101` |
| Left half of SW result | `1001` |
| EP(right half) | `11101011` |
| XOR with K2 (`01000011`) | `10101000` |
| S0 on left nibble `1010`: row `10`=2, col `01`=1 → output `10` | |
| S1 on right nibble `1000`: row `10`=2, col `00`=0 → output `11` | |
| S0‖S1 | `1011` |
| P4(S0‖S1) | `0111` |
| XOR with left half (`1001`) | `1110` |
| Result = XOR-output ‖ right half | `11101101` (end of round 2) |
| IP⁻¹(Result) | `01110111` |

**Ciphertext = `01110111`** (= `0x77` = `'w'` in ASCII).

---

## What to remember for the exam

S-DES mirrors DES exactly in structure — IP, Feistel-style rounds with subkeys, swap between rounds, IP⁻¹ — just scaled down to 8-bit blocks, a 10-bit key, and 2 rounds, so it's small enough to trace by hand. Subkey generation: P10 → split → shift(s) → P8. Each round: expand-permute the right half (EP), XOR with the round key, run each nibble through an S-box, permute (P4), XOR with the left half, recombine with the (unchanged) right half.

---

## Exam check

1. What are S-DES's block size, key size, and number of rounds?
2. Describe the subkey generation pipeline from the 10-bit key to K1 and K2.
3. Walk through what happens inside one round of S-DES encryption, in order.
4. How does decryption differ from encryption in S-DES?
