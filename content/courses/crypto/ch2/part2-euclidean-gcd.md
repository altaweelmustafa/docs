---
title: "Chapter 2, Part 2 – The Euclidean Algorithm & GCD"
date: 2026-08-09
description: "Greatest common divisor, relatively prime integers, and the Euclidean algorithm for computing GCD efficiently."
tags: [information-security, number-theory, euclidean-algorithm, gcd, chapter2]
toc: true
weight: 2
---

## Greatest common divisor (GCD)

`gcd(a, b)` is the largest integer that divides both `a` and `b`. Formally, positive integer `c = gcd(a, b)` if:

1. `c` divides both `a` and `b`, and
2. any divisor of both `a` and `b` also divides `c`.

Equivalently: `gcd(a, b) = max{k : k | a and k | b}`.

Special cases and identities:

- `gcd(0, 0) = 0` (defined by convention).
- Because GCD must be positive: `gcd(a, b) = gcd(a, -b) = gcd(-a, b) = gcd(-a, -b) = gcd(|a|, |b|)`.
- `gcd(60, 24) = gcd(60, -24) = 12`.
- Since every non-zero integer divides 0: `gcd(a, 0) = |a|`.

### Relatively prime

Two integers `a` and `b` are **relatively prime** if their only common positive divisor is 1 — equivalently, `gcd(a, b) = 1`.

**Example:** 8 and 15 are relatively prime. Divisors of 8: {1, 2, 4, 8}. Divisors of 15: {1, 3, 5, 15}. Only 1 appears in both lists.

---

## The Euclidean algorithm

Repeatedly applying the division algorithm gives an efficient way to compute GCD, without factoring either number:

```
Start: a > b?
  No  → swap a and b
  Yes → divide a by b, remainder r
        r > 0?
          Yes → a ← b, b ← r, repeat the division
          No  → GCD is the current value of b. Done.
```

### Worked example: gcd(710, 310)

```
710 = 2 × 310 + 90
310 = 3 ×  90 + 40
 90 = 2 ×  40 + 10
 40 = 4 ×  10 +  0     ← remainder hits 0
```

The last non-zero remainder is **10**, so `gcd(710, 310) = 10`.

The algorithm converges fast even on large numbers — a 10-digit example (`gcd(1160718174, 316258250)`) still finishes in 10 steps, ending at a remainder of **1078**.

> Exam sentence: the Euclidean algorithm's key insight is `gcd(a, b) = gcd(b, a mod b)` — repeatedly replace the pair with `(b, a mod b)` until the remainder is 0; the last non-zero remainder is the GCD.

---

## What to remember for the exam

GCD via factoring is slow for large numbers; the Euclidean algorithm gets there in a handful of division steps by always working with `(b, a mod b)`. Relatively prime just means `gcd = 1`.

---

## Exam check

1. Compute `gcd(1024, 936)` using the Euclidean algorithm, showing every step.
2. Why does the Euclidean algorithm terminate?
3. Are 35 and 64 relatively prime? Justify your answer.
