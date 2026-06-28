---
title: "Chapter 8, Part 3 – Segmentation & Segmentation with Paging"
date: 2026-05-29
weight: 3
toc: true
tags: ["os", "memory-management", "segmentation", "paging"]
description: "Segmentation as a user-view memory model, the segment table, address translation, and combining segmentation with paging."
---

## Segmentation

Paging separates the user's view of memory from actual memory. **Segmentation** preserves the user's logical view: a program is a collection of named segments (main program, stack, data structures, functions/procedures).

Each segment has a **segment number (S)** and a **displacement/offset (d)**.

**Segment Table** stores `(Base, Limit)` per segment:

```
LA → [ S | d ]
PA  = Base[S] + d        (if d < Limit[S], else → fault)
```

**Example:**

| Seg# | Base   | Limit |
| ---- | ------ | ----- |
| 0    | 27,000 | 7,000 |
| 1    | 1,000  | 5,000 |
| 2    | 18,000 | 3,000 |
| 3    | 21,000 | 2,000 |

Given LA with `S = 2, d = 350`:

```
PA = 18,000 + 350 = 18,350
```

---

## Segmentation with Paging

_"Paging the segments"_ — combine both schemes.

Each segment is divided into pages. The segment table now points to a **page table** for that segment instead of a direct base address.

```
LA → [ S | P | d ]
         ↓
  Segment table[S] → address of page table
         ↓
  Page table[P] → Frame F
         ↓
  PA = F × Page Size + d
```

**Example:** If segment size = 64 KB and page size = 4 KB, each segment contains 16 pages.

This scheme gives the flexibility of segmentation (logical units) with the non-contiguous placement advantage of paging.
