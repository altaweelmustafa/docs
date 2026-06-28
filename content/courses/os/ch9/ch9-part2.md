---
title: "Chapter 9, Part 2 – Frame Allocation, Global vs. Local Replacement & Thrashing"
date: 2026-05-29
weight: 2
toc: true
tags: ["os", "virtual-memory", "frame-allocation", "thrashing"]
description: "How frames are allocated to processes, global vs. local page replacement, and the thrashing problem."
---

## Allocation of Frames to Processes

How many frames does each process get?

### (1) Equal Allocation

Every process gets the same number of frames.

**Example:** 100 frames, 5 processes → each gets `100 / 5 = 20 frames`.

**Problem:** Unfair and poor performance — a small process wastes frames, a large one starves.

---

### (2) Proportional Allocation (by Size)

Let `Si` = size of process `Pi`, `M` = total frames in memory.

```
Frames for Pi = (Si / ΣSi) × M
```

**Example:** M = 100 frames, 3 processes with sizes 100, 400, 700 KB:

```
P1 = (100/1200) × 100 ≈  8 frames
P2 = (400/1200) × 100 ≈ 34 frames
P3 = (700/1200) × 100 ≈ 58 frames
```

---

### (3) Proportional Allocation (by Priority)

Same formula but using **priority** instead of size. Higher priority → more frames.

**Example:** 100 frames, priorities 2, 3, 7:

```
P1 = (2/12) × 100 ≈ 17 frames
P2 = (3/12) × 100 = 25 frames
P3 = (7/12) × 100 ≈ 58 frames
```

---

## Global vs. Local Replacement

- **Global Replacement:** A process can steal a frame from _any other process_. More flexible, commonly used.
- **Local Replacement:** A process can only replace its _own_ frames. More predictable but can underutilize memory.

---

## Thrashing

**Thrashing** occurs when the OS is spending most of its time swapping pages in and out rather than executing processes.

**Why it happens:**

1. A process has too few frames allocated → high page fault rate.
2. OS sees low CPU utilization → increases degree of multiprogramming (adds more processes).
3. Each new process takes frames from others → all processes fault more.
4. The system spirals: OS is almost entirely doing page swapping.

**Graph:** Page faults vs. allocated frames — performance plummets at low frame counts, then levels off as frames increase. The "poor performance" zone is thrashing.

**Solution:** Reduce the degree of multiprogramming or use a working-set model to allocate enough frames for each process's active pages.
