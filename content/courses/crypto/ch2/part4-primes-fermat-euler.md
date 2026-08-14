---
title: "Chapter 2, Part 4 – Prime Numbers, Fermat's & Euler's Theorems"
date: 2026-08-09
description: "The fundamental theorem of arithmetic, and the two theorems public-key cryptography relies on: Fermat's little theorem and Euler's theorem."
tags: [information-security, number-theory, primes, fermat, euler, chapter2]
toc: true
weight: 4
---

## Prime numbers

An integer `p > 1` is **prime** if and only if its only divisors are `±1` and `±p`. Primes are central to number theory and to everything that follows in public-key cryptography.

### Fundamental theorem of arithmetic

Every integer `a > 1` factors **uniquely** as:

```
a = p1^a1 × p2^a2 × ... × pt^at
```

where `p1 < p2 < ... < pt` are primes and each `ai` is a positive integer. In other words, every integer greater than 1 has one and only one prime factorization (up to ordering).

---

## Fermat's little theorem

If `p` is prime and `a` is a positive integer **not divisible by `p`**, then:

```
a^(p-1) ≡ 1 (mod p)
```

**Alternative form** (holds for *any* positive integer `a`, divisible by `p` or not):

```
a^p ≡ a (mod p)
```

This theorem plays a critical role in public-key algorithms and primality testing.

---

## Euler's theorem

For every `a` and `n` that are **relatively prime**:

```
a^φ(n) ≡ 1 (mod n)
```

where `φ(n)` is **Euler's totient function** — the count of positive integers less than `n` that are relatively prime to `n`.

**Alternative form:**

```
a^(φ(n)+1) ≡ a (mod n)
```

Euler's theorem is a generalization of Fermat's: when `n = p` is prime, `φ(p) = p - 1`, and Euler's theorem reduces exactly to Fermat's little theorem.

> Exam sentence: Fermat's theorem is a special case of Euler's theorem for prime moduli — Fermat needs `p` prime, Euler only needs `gcd(a, n) = 1`.

---

## Why this matters

These two theorems are the mathematical backbone of asymmetric cryptography covered later in the course — most directly, RSA's correctness proof relies on Euler's theorem (with `n` a product of two primes, so `φ(n) = (p-1)(q-1)`).

---

## What to remember for the exam

Fundamental theorem of arithmetic: unique prime factorization. Fermat's little theorem needs a prime modulus; Euler's theorem generalizes it to any modulus, as long as the base is relatively prime to the modulus.

---

## Exam check

1. State Fermat's little theorem and its alternate form.
2. State Euler's theorem, and explain why it generalizes Fermat's.
3. Why must `a` and `n` be relatively prime for Euler's theorem to apply?
