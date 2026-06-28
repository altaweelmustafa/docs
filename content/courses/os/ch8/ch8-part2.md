---
title: "Chapter 8, Part 2 – Non-Contiguous Allocation: Paging"
date: 2026-05-29
weight: 2
toc: true
tags: ["os", "memory-management", "paging", "page-table", "TLB"]
description: "Paging: dividing programs into pages and memory into frames, the page table, address translation, TLB, and memory protection."
---

## [2] Non-Contiguous Allocation — Paging

- The logical program is divided into equal-size chunks called **pages**.
- Physical memory is divided into equal-size chunks called **frames** (same size as pages).
- Pages can be placed into any free frames — no need for contiguous memory.

---

## Address Translation

The OS uses a **page table**: a table that maps each page number (P) to its frame number (F).

**Logical Address layout:**

```
LA → [ P | d ]
      page#  offset
```

**Translation formulas:**

```
P  = LA / Page Size
d  = LA % Page Size
PA = F × Page Size + d
```

**Example** (Page Size = 100 bytes, LA = 271):

```
P = 271 / 100 = 2
d = 271 % 100 = 71
→ Page table says page 2 is in frame 1012
PA = 1012 × 100 + 71 = 101,271
```

> **Note:** `/` and `%` are multiplication-class operations — they take time. In practice, page size is always a power of 2 (typically `4096 = 2¹²`), so the CPU extracts `d` from the **low-order n bits** and `P` from the remaining high-order bits — no division needed.

---

## Page Table Size Problem

For large address spaces the page table itself gets enormous:

| LA bits | Page Size  | Page Table Size        |
| ------- | ---------- | ---------------------- |
| 32 bits | 4 KB (2¹²) | 2²⁰ × 4 B = **4 MB**   |
| 40 bits | 4 KB       | 2²⁸ × 4 B = **1 GB**   |
| 48 bits | 4 KB       | 2³⁶ × 4 B = **256 GB** |

---

## Where Is the Active Page Table Stored?

Three options:

### (1) Registers

Works only if the page table is very small (e.g., PDP-11: LA = 16 bits, Page Size = 8192 = 2¹³ → only 2³ = 8 pages, table = 32 bytes).

### (2) Memory — PTBR + PTLR

Keep the page table in main memory; identify it with the **Page Table Base Register (PTBR)** and **Page Table Limit Register (PTLR)**.

**Problem:** Every instruction now requires **two memory accesses** — one to look up the page table, one to fetch the actual instruction/data.

### (3) Memory + Registers — TLB (Associative Registers)

A small, fast set of registers called **Translation Look-aside Buffers (TLB)** caches recently used `(Page#, Frame#)` pairs.

- If the page is in the TLB (**hit**): PA computed in one step.
- If not (**miss**): fall back to the page table in memory.

**Effective Access Time (EAT):**

```
EAT = h × (m + t) + (1 − h) × (2m + t)
```

Where `h` = hit ratio, `m` = memory access time, `t` = TLB search time.

**Example:** m = 100 ns, t = 1 ns, h = 0.95 → EAT = 108 ns.

---

## Multi-Level Page Tables

Since the page table for a 32-bit LA is 4 MB (too big to keep contiguous), we can split it into **levels**:

**Two-level (LA = 32 bits, Page Size = 4 KB):**

```
LA → [ P1 (8 bits) | P2 (12 bits) | d (12 bits) ]
```

- Outer page table size: 2⁸ × 4 = 1 KB
- Inner page table size: 2¹² × 4 = 16 KB

**Four-level (LA = 48 bits):**

```
LA → [ P1(6) | P2(10) | P3(10) | P4(10) | d(12) ]
```

**Problem:** More levels = more memory accesses. With good TLB coverage, performance stays acceptable.

---

## Memory Protection in Paging

Additional bits are stored in each page table entry:

- **Legal/Illegal Bit:** `1` = legal page, `0` = illegal (accessing triggers a fault).
- **R/W Bit:** `1` = read/write page, `0` = read-only page.

---

## Paging Advantages — Shared Pages

Two processes can map different page table entries to the **same frame** in physical memory (e.g., shared library code). Only one copy of the code lives in memory.

**Disadvantage:** Some feel the program being split into many pieces in memory is a concern (mostly philosophical).
