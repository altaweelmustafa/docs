---
title: "Chapter 8, Part 1 – Memory Management: Basics & Contiguous Allocation"
date: 2026-05-29
weight: 1
toc: true
tags: ["os", "memory-management", "contiguous-allocation", "binding"]
description: "Ordinary memory management, logical vs physical addresses, binding times, and contiguous (fixed/dynamic) partitions."
---

## What is Memory Management?

**Ordinary Memory Management** means all programs must be admitted (allocated) into memory **before** execution starts.

---

## Logical Address vs. Physical Address

- **Logical Address (LA):** The address seen in your program — it's the offset of the address within the program.
- **Physical Address (PA):** The actual address in memory.

The OS maps logical addresses to physical ones at runtime:

```
PA = LA + Base Register
Example: PA = 172 + 21568 = 21740
```

---

## Binding Times

When does the OS determine physical addresses?

1. **At Compilation Time** — PAs are assigned at the beginning. The program must be loaded at the same memory location every time and cannot change location during execution.
2. **At Loading Time** — PAs are decided when the program is loaded into memory. Problem: the program still can't be moved during execution.
3. **At Execution Time** _(Best)_ — The most flexible. Our objective is to compute the PA and give it to the CPU for instruction fetch. This is the basis of modern memory management.

---

## [1] Contiguous Allocation – Multiple Partitions

Memory is divided into partitions (regions). Every region holds only one process. When a region becomes free, a new program loads into it.

**Hardware support needed:** Base Register + Limit Register.

```
PA = LA + Base Register
```

The CPU checks: if `LA < Limit Register` → compute PA; otherwise → **memory fault**.

### (1) Fixed Regions — IBM MFT

Memory is divided into a **fixed number of partitions** with fixed sizes. The degree of multiprogramming is bounded by the number of regions.

**Job Scheduling** — how does the OS select a job for a region?

- **(a)** Each region has its own queue of waiting jobs.
- **(b)** One shared queue for all jobs.

Scheduling policies: FCFS (with or without skip), Best Fit, Best Available Fit.

**Problem: Internal Fragmentation** — unused memory _inside_ a region (the job is smaller than the partition).

**External Fragmentation** — an unused region too small to fit any available job.

### (2) Dynamic (Variable) Regions

Partitions are created dynamically to fit the size of each arriving job. Over time, memory ends up as a mix of allocated regions and **holes** (external fragmentation).

**Job Scheduling — How to select a hole for a process:**

- **(1) First Fit** — pick the first hole big enough.
- **(2) Best Fit** — pick the smallest hole that fits.
- **(3) Worst Fit** — pick the largest hole.

**Problem: External Fragmentation** → **Solution: Compaction** (move all processes together to merge holes).
