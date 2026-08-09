---
title: "Chapter 2, Part 1 – Divisibility & the Division Algorithm"
date: 2026-08-09
description: "Basic divisibility properties and the division algorithm a = qn + r."
tags: [information-security, number-theory, divisibility, chapter2]
toc: true
weight: 1
---

## Divisibility

We say a non-zero integer **b divides a** (written `b | a`) if `a = mb` for some integer `m` — i.e. there's no remainder.

Useful properties to know cold:

- If `a | 1`, then `a = ±1`.
- If `a | b` and `b | a`, then `a = ±b`.
- Any non-zero `b` divides 0.
- If `a | b` and `b | c`, then `a | c` (divisibility is transitive). Example: `11 | 66` and `66 | 198`, so `11 | 198`.
- If `b | g` and `b | h`, then `b | (mg + nh)` for **any** integers `m, n`.

### Why the last property holds

If `b | g`, then `g = b·g₁` for some integer `g₁`. If `b | h`, then `h = b·h₁` for some integer `h₁`. So:

```
mg + nh = m(bg₁) + n(bh₁) = b(mg₁ + nh₁)
```

which is clearly divisible by `b`.

**Worked example:** `b = 7, g = 14, h = 63, m = 3, n = 2`. Since `7 | 14` and `7 | 63`:

```
3·14 + 2·63 = 42 + 126 = 168 = 7 × 24
```

so `7 | (3·14 + 2·63)`, confirming the property.

---

## The division algorithm

For any positive integer `n` and non-negative integer `a`, dividing `a` by `n` gives a unique quotient `q` and remainder `r`:

```
a = qn + r        0 ≤ r < n,   q = ⌊a/n⌋
```

`r` is often called the **residue**. Visually (Figure 2.1), mark 0, n, 2n, 3n, … on a number line; `a` falls somewhere past `qn` and before `(q+1)n`, and the distance from `qn` to `a` is `r`.

**Examples:**

- `a = 11, n = 7` → `11 = 1×7 + 4` → `q = 1, r = 4`
- `a = -11, n = 7` → `-11 = (-2)×7 + 3` → `q = -2, r = 3` (note `r` is always non-negative, even for negative `a`)

> Exam sentence: the division algorithm always produces a *non-negative* remainder strictly less than `n`, regardless of the sign of `a` — this is what makes modular arithmetic well-defined later in the chapter.

---

## What to remember for the exam

Divisibility properties (especially the linear-combination property) are the toolwork behind the Euclidean algorithm in Part 2. The division algorithm's remainder is always in `[0, n)`.

---

## Exam check

1. State and prove that if `b | g` and `b | h`, then `b` divides any integer linear combination `mg + nh`.
2. Compute `q` and `r` for `a = -23, n = 5` using the division algorithm.
3. Why must `r` satisfy `0 ≤ r < n` rather than `-n < r < n`?
