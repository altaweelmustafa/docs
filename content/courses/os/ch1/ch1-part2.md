---
title: "Chapter 1, Part 2 – Offline Operation, Buffering, Spooling & System Types"
date: 2026-06-28
weight: 2
toc: true
tags:
  [
    "operating-systems",
    "buffering",
    "spooling",
    "multiprogramming",
    "timesharing",
    "parallel-systems",
    "real-time",
  ]
description: "Solutions to early OS inefficiency: offline operation, buffering, spooling, multiprogramming, timesharing, and parallel/real-time systems."
author: "Mustafa Altaweel"
---

## Improving CPU Utilization

The key problem was: **CPU is fast, I/O is slow**. The CPU executes until it reaches an I/O instruction (e.g. printing or reading input), then sits idle. Several techniques were developed to fix this.

---

## [A] Offline Operation

### Before (Online):

```
Card Reader → Input → CPU → Output → Printer
```

The card reader fed data directly to the CPU.

### After (Offline):

```
Card Reader → [Tape] → CPU → [Tape] → Printer
             offline prep           offline prep
```

Data was first written to **tape** offline (preparation step), then the CPU read from tape instead of the card reader directly.

> **Key insight**: Tape-to-memory is **much faster** than card-reader-to-memory → improves execution speed.

---

## [B] Buffering

In buffering, **input and output buffers** are introduced in memory. When the CPU reaches an I/O instruction, it reads data from the **input buffer** (already loaded by the OS), not from the card reader directly.

**How it works:**

1. Card reader sends data → Input Buffer (in memory).
2. CPU reads from Input Buffer (fast).
3. CPU sends results → Output Buffer.
4. Output Buffer sends to Printer.

> **Conclusion**: The I/O of one job is **overlapped with the execution of the same job**.

---

## [C] Spooling

**Spooling** introduced two data structures:

1. **Job Queue (Job Pool)** — a queue containing all jobs (programs) that demand execution.
2. **Spool Area** — contains the jobs that need printing (output).

**Spooling flow:**

```
Input Device → Job Queue / Spool Area (on disk) → Memory → Output Device
```

> **Conclusion**: The I/O of one job is overlapped with the **execution of another job** (not the same job as in buffering). This is a big improvement.

---

## [D] Multiprogramming Batch Systems

**"Multi-programming"**

- Memory is divided into several **regions (partitions)**.
- Region sizes are normally different.
- Each region contains **only one job**.
- **The CPU switches to another job when the first one needs I/O.**

```
Memory:
  ┌──────────┐
  │    OS    │
  ├──────────┤
  │  Job 1   │ ← currently running
  ├──────────┤
  │  Job 2   │ ← waiting
  ├──────────┤
  │  Job 3   │ ← waiting
  └──────────┘
```

> **Key rule**: Any job (process/program) is a sequence of **CPU bursts** and **I/O waits**, and it must start and end with a CPU burst.

### Two Kinds of Jobs

| Type              | Description                                                                 |
| ----------------- | --------------------------------------------------------------------------- |
| **CPU-bound job** | Contains few, very long CPU bursts. Most of the time the job needs the CPU. |
| **I/O-bound job** | Contains many, very short CPU bursts. Most of the time the job needs I/O.   |

### Context Switch

When the CPU switches from Job 1 to Job 2:

- **Saves** the register state for Job 1.
- **Reloads** the register state for Job 2.

---

## [E] Time Sharing Systems

Same idea as multiprogramming, with one key difference:

- Memory is divided into regions; several jobs are kept in memory.
- Each job is assigned a **time slice** called **quantum Q**.
- The job executes for its quantum, then the CPU switches to the next job.

**The CPU also switches early if:**

- The job needs I/O.
- The job finishes execution.
- A higher-priority process arrives.

### Multiprogramming vs. Time Sharing

| Feature           | Multiprogramming | Time Sharing                          |
| ----------------- | ---------------- | ------------------------------------- |
| CPU switches when | Job needs I/O    | Quantum Q is finished (or I/O needed) |
| Goal              | CPU utilization  | Fast response time (interactive)      |

---

## [F] Parallel Systems

**Multi-processor systems** with more than one CPU in close communication.

### (1) Tightly Coupled Systems

Processors share memory, clock, and communication takes place in memory.

**Two types of multiprocessing:**

**a. Symmetric Multiprocessing (SMP)**

- Each CPU has the **same identical copy** of the OS.
- Reliable and simple.

**b. Asymmetric Multiprocessing (AMP)**

- There is one **Master CPU** which controls all other CPUs.
- Relation between master and others = **master/slave relationship**.
- Reliable in all cases _unless_ the master CPU is faulty.

### (2) Loosely Coupled Systems

- **Networks** — a network-based OS where systems connect via servers.
- Also called **Distributed Systems**.

---

## [G] Real-Time Systems

- Takes data using **sensors**.
- Used for systems that need an **immediate response** (real-time / استجابة فورية).
- Examples: medical equipment, weapons systems, industrial controllers, traffic signals.
