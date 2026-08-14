---
title: "Chapter 2, Part 3 – Modular Arithmetic"
date: 2026-08-09
description: "The modulus, congruence, properties of congruences, and the algebraic properties of arithmetic in Zn."
tags: [information-security, number-theory, modular-arithmetic, chapter2]
toc: true
weight: 3
---

## The modulus

For integer `a` and positive integer `n`, `a mod n` is defined as the remainder when `a` is divided by `n`. `n` is called the **modulus**.

```
a = qn + r        0 ≤ r < n,   q = ⌊a/n⌋
a = ⌊a/n⌋·n + (a mod n)
```

**Examples:** `11 mod 7 = 4`; `-11 mod 7 = 3` (remember: the result of `mod` is always non-negative).

---

## Congruence

Two integers `a` and `b` are **congruent modulo n** if `(a mod n) = (b mod n)`, written `a ≡ b (mod n)`.

If `a ≡ 0 (mod n)`, then `n | a`.

**Examples:** `73 ≡ 4 (mod 23)`; `21 ≡ -9 (mod 10)`.

### Properties of congruence

1. `a ≡ b (mod n)` if `n | (a − b)`.
2. `a ≡ b (mod n)` implies `b ≡ a (mod n)` (symmetric).
3. `a ≡ b (mod n)` and `b ≡ c (mod n)` implies `a ≡ c (mod n)` (transitive).

**Worked examples:**

- `23 ≡ 8 (mod 5)` because `23 − 8 = 15 = 5 × 3`.
- `-11 ≡ 5 (mod 8)` because `-11 − 5 = -16 = 8 × (-2)`.
- `81 ≡ 0 (mod 27)` because `81 − 0 = 81 = 27 × 3`.

---

## Modular arithmetic exhibits familiar structure

```
[(a mod n) + (b mod n)] mod n = (a + b) mod n
[(a mod n) − (b mod n)] mod n = (a − b) mod n
[(a mod n) × (b mod n)] mod n = (a × b) mod n
```

**Worked example** (`a = 11, b = 15, n = 8`):

| Expression | Value |
|---|---|
| `11 mod 8`, `15 mod 8` | `3`, `7` |
| `[(11 mod 8) + (15 mod 8)] mod 8` | `10 mod 8 = 2` |
| `(11 + 15) mod 8` | `26 mod 8 = 2` ✓ |
| `[(11 mod 8) − (15 mod 8)] mod 8` | `-4 mod 8 = 4` |
| `(11 − 15) mod 8` | `-4 mod 8 = 4` ✓ |
| `[(11 mod 8) × (15 mod 8)] mod 8` | `21 mod 8 = 5` |
| `(11 × 15) mod 8` | `165 mod 8 = 5` ✓ |

### Zn as a commutative ring

Working within `Zn = {0, 1, ..., n-1}`, arithmetic obeys:

| Property | Expression |
|---|---|
| Commutative | `(w + x) mod n = (x + w) mod n`, same for `×` |
| Associative | `[(w + x) + y] mod n = [w + (x + y)] mod n`, same for `×` |
| Distributive | `[w × (x + y)] mod n = [(w×x) + (w×y)] mod n` |
| Identities | `(0 + w) mod n = w mod n`, `(1 × w) mod n = w mod n` |
| Additive inverse | for every `w ∈ Zn`, some `z` exists with `w + z ≡ 0 (mod n)` |

This makes `Zn` a **commutative ring with a multiplicative identity**.

**Multiplicative inverses are special:** an integer has a multiplicative inverse in `Zn` only if it's relatively prime to `n`. In `Z8`: 1, 3, 5, 7 have inverses; 2, 4, 6 do not (they share a factor with 8).

> Exam sentence: additive inverses always exist in `Zn`; multiplicative inverses exist only for elements relatively prime to `n`.

---

## What to remember for the exam

`a mod n` is always in `[0, n)`. Congruence is an equivalence relation (reflexive via property 2+3, symmetric, transitive). Addition, subtraction, and multiplication all "commute" with mod. Multiplicative inverses in `Zn` exist only for values coprime to `n`.

---

## Exam check

1. Show that `[(a mod n) × (b mod n)] mod n = (a × b) mod n` using the definitions `a mod n = ra`, `b mod n = rb`.
2. Which elements of `Z12` have a multiplicative inverse?
3. Is `40 ≡ 4 (mod 9)`? Show your work.
