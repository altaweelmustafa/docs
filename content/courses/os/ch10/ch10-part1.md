---
title: "Chapter 10 – Disk Scheduling"
date: 2026-05-29
weight: 1
toc: true
tags:
  ["os", "disk-scheduling", "hard-disk", "seek-time", "FCFS", "SSTF", "SCAN"]
description: "Hard disk structure, access time components, and disk scheduling algorithms: FCFS, SSTF, SCAN, C-SCAN, LOOK, C-LOOK."
---

## Hard Disk Structure

A hard disk address is `(track#, surface#, block#)` — e.g., `(225, 3, 17)`.

**HD Access Time = Seek Time + Latency Time + Transfer Time**

- **Seek Time:** Time for the R/W head to move to the correct track (mechanical — the dominant cost).
- **Latency Time:** Time for the disk to rotate to the correct sector.
- **Transfer Time:** Time to actually read/write the data.

We have no control over latency or transfer time, but we **can reduce seek time** by choosing a smart disk scheduling algorithm.

---

## Disk Scheduling Algorithms

**Setup for all examples:**

- HD has 200 tracks (0–199)
- Request queue: `98, 183, 37, 122, 14, 124, 65, 67`
- R/W head currently at track **53**, previously at track **40** (moving right)

---

### (1) FCFS — First Come, First Served

Serve requests in arrival order.

Movement: `53 → 98 → 183 → 37 → 122 → 14 → 124 → 65 → 67`

**Average Head Movement (AHM) ≈ 98 tracks/job** — very high, lots of back-and-forth.

---

### (2) SSTF — Shortest Seek Time First

Always serve the request **closest to the current head position**.

Movement: `53 → 65 → 67 → 37 → 14 → 98 → 122 → 124 → 183`

**AHM ≈ 29 tracks/job** — optimal solution (minimum).

**Problem:** Starvation — requests far from the head may wait forever.

---

### (3) SCAN (Elevator Algorithm)

The head moves in one direction, serving all requests along the way, then **reverses** at the end.

Movement: `53 → 65 → 67 → 98 → 122 → 124 → 183 → 199 → 37 → 14`

**AHM ≈ 41 tracks/job**

---

### (4) C-SCAN (Circular SCAN)

Like SCAN but after reaching one end, the head jumps back to the **beginning** without serving on the return trip.

Movement: `53 → 65 → 67 → 98 → 122 → 124 → 183 → 199 → (jump to 0) → 14 → 37`

**AHM ≈ 47 tracks/job** — more uniform wait times than SCAN.

---

### (5) LOOK

Like SCAN but the head only goes as far as the **last request** in each direction (doesn't travel to the physical end of the disk).

Movement: `53 → 65 → 67 → 98 → 122 → 124 → 183 → (reverse) → 37 → 14`

**AHM ≈ 37 tracks/job**

---

### (6) C-LOOK (Circular LOOK)

Like C-SCAN but jumps back to the **first pending request** instead of track 0.

Movement: `53 → 65 → 67 → 98 → 122 → 124 → 183 → (jump to 14) → 14 → 37`

**AHM ≈ 42 tracks/job**

---

## Summary

| Algorithm | AHM (example) | Notes                               |
| --------- | ------------- | ----------------------------------- |
| FCFS      | ~98           | Simple, poor performance            |
| SSTF      | ~29           | Optimal seek, risk of starvation    |
| SCAN      | ~41           | Good balance, no starvation         |
| C-SCAN    | ~47           | Uniform wait times                  |
| LOOK      | ~37           | Better than SCAN (no wasted travel) |
| C-LOOK    | ~42           | Better than C-SCAN                  |
