---
title: "Chapter 9, Part 1 – Virtual Memory: Demand Paging & Page Replacement"
date: 2026-05-29
weight: 1
toc: true
tags:
  ["os", "virtual-memory", "demand-paging", "page-fault", "page-replacement"]
description: "Virtual memory concepts, demand paging, valid/invalid bits, page faults, and page replacement algorithms (FIFO, Optimal, LRU)."
---

## Virtual Memory Management

Virtual memory allows running programs **larger than physical memory** by loading only the needed parts.

- No need for the entire program to be in memory — only the active pages are required.
- Main mechanism: **Demand Paging**.

---

## Demand Paging

The page table gains a **Valid/Invalid (V/I) bit** per entry:

- `1` = page is in memory (valid)
- `0` = page is NOT in memory (invalid)

When the CPU accesses a page with V/I = 0, a **page fault** occurs:

1. OS looks for a free frame in memory.
2. Swaps the required page **in** from disk (HDD).
3. Updates the page table (set frame number, V/I = 1).
4. Resumes execution.

If there is **no free frame**, the OS selects a **victim frame**, swaps it **out** to disk, then swaps the required page in.

---

## Performance of Demand Paging

Let:

- `P` = page fault rate (`0 ≤ P ≤ 1`)
- `M` = memory access time
- Swap time = page fault overhead

```
EAT = (1 − P) × M + P × [swap-in + maybe swap-out + M]
```

**Example:** M = 10 µs, swap time = 10 ms, 40% of faults need swap-out:

```
EAT = (1−P)×10 + P×[10,000 + 0.4×10,000 + 10]
    ≈ 10 + 14,000P
```

**Objective: Minimize page fault rate P.**

> **Note:** A "dirty bit" is added to the page table. If `dirty = 1`, the page was modified and must be written to disk on swap-out. If `dirty = 0`, skip the write — saves time.

---

## Page Replacement Algorithms

The OS must pick the **victim frame** wisely to keep `P` low.

### [1] FIFO — First In, First Out

Replace the page that entered memory **first** (oldest page).

**Example** (3 frames, reference string: `1 2 3 4 1 2 5 1 2 3 4 5`):

- **9 page faults**

**Problem:** Adding more frames can _increase_ page faults → **Belady's Anomaly**.

---

### [2] Optimal Replacement

Replace the page that will **not be used for the longest time** in the future.

Same example:

- **7 page faults** (minimum possible)

**Major problem:** The OS cannot know the future. Used only as a **benchmark** to evaluate other algorithms.

---

### [3] LRU — Least Recently Used

Replace the page that **has not been used for the longest time in the past**.

Same example:

- **10 page faults** (3 frames) / **8 page faults** (4 frames)

LRU has no Belady's Anomaly.

**Implementation options:**

1. **Counter:** Add a timestamp field to each page table entry recording the last access time. At replacement, pick the entry with the oldest timestamp.

2. **Queue/Stack:** Every time a page is used, move it to the front of a queue (or push to top of stack). The back of the queue / bottom of stack = LRU victim.

---

### LRU Approximation — Reference Bit

Add a **reference bit** to each page table entry:

- `1` = page was referenced (read or written) recently
- `0` = page has not been referenced

**LRU approximation:** replace the page whose reference bit is `0`.

#### Second Chance (Clock Algorithm)

Pages are arranged in a circular queue. The pointer moves counter-clockwise:

- Reference bit = `1` → clear to `0`, skip (give it a second chance).
- Reference bit = `0` → **replace** this page.

#### Enhanced Second Chance

Uses both (reference bit, dirty bit):

| (ref, dirty) | Meaning                      | Priority to replace |
| ------------ | ---------------------------- | ------------------- |
| (0, 0)       | Not referenced, not modified | **Best** ①          |
| (1, 0)       | Referenced, not modified     | ②                   |
| (0, 1)       | Not referenced, but modified | ③                   |
| (1, 1)       | Referenced and modified      | **Worst** ④         |

---

### Counting Algorithms

- **MFU (Most Frequently Used):** Replace the page used the **most** times — reasoning: it's probably done.
- **LFU (Least Frequently Used):** Replace the page used the **least** times — it was barely needed.
