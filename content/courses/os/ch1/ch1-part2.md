---
title: "Chapter 1, Part 2 – Computer-System Architecture & Operating-System Structure"
date: 2026-06-28
weight: 2
toc: true
tags:
  [
    "operating-systems",
    "multiprocessor",
    "clustered-systems",
    "multiprogramming",
    "timesharing",
  ]
description: "Single-processor vs. multiprocessor vs. clustered systems, and how multiprogramming and timesharing shape OS structure."
author: "Mustafa Altaweel"
---

## Computer-System Architecture

### Single-Processor Systems

Most systems historically used a **single general-purpose processor**. They also contain special-purpose processors (disk, keyboard, graphics) that run a limited instruction set and do not run user processes.

### Multiprocessor Systems

Also called **parallel systems** or **tightly coupled systems**. They have two or more processors sharing a computer bus, clock, memory, and peripheral devices.

**Advantages:**

1. **Increased throughput** — more work done in less time. Speedup ratio is less than `N` for `N` processors due to coordination overhead.
2. **Economy of scale** — cheaper than equivalent separate systems since resources are shared.
3. **Increased reliability** — failure of one processor slows but doesn't halt the system (**graceful degradation** / **fault tolerance**).

**Two types:**

| Type                                 | Description                                                                                              |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------- |
| **Asymmetric Multiprocessing (AMP)** | A boss processor controls the system and assigns work to worker processors.                              |
| **Symmetric Multiprocessing (SMP)**  | All processors are peers; each runs its own copy of the OS and can perform all tasks. Most common today. |

In SMP, all CPUs share one physical address space. The number of processes that can run simultaneously scales with the processor count.

### Multi-Core Systems

Multiple computing cores reside on a single chip. On-chip communication is faster than between-chip communication, and a multi-core chip uses less power than multiple single-core chips.

### Clustered Systems

Clustered systems couple **multiple independent systems (nodes)** over a LAN or InfiniBand. They share storage (usually via a **storage-area network, SAN**) and provide **high availability**.

**Two types:**

- **Asymmetric clustering**: one machine is in hot-standby mode, monitoring the active server. If the active server fails, the standby takes over (**failover**).
- **Symmetric clustering**: multiple nodes run applications and monitor each other. More efficient since all hardware is in use.

Some clusters are designed for **high-performance computing (HPC)**. Applications must be written to use **parallelization**. Clusters can also implement **distributed lock manager (DLM)** to avoid conflicting operations across nodes.

---

## Operating-System Structure

### Multiprogramming

A single user cannot keep the CPU and I/O devices busy at all times. **Multiprogramming** solves this by organizing multiple jobs so the CPU always has something to execute.

- A subset of all jobs is kept in memory.
- One job is selected and run via **job scheduling**.
- When a job must wait (e.g., for I/O), the OS switches to another job.
- The CPU is never idle as long as at least one job needs to execute.

### Timesharing (Multitasking)

**Timesharing** is a logical extension of multiprogramming in which the CPU switches jobs so frequently that users can interact with each job while it is running, creating **interactive computing**.

Key characteristics:

- Response time should be **< 1 second**.
- Each user has at least one program executing in memory — this is called a **process**.
- If several processes are ready to run simultaneously → **CPU scheduling**.
- If processes do not fit in memory → **swapping** (moving processes in and out of memory).
- **Virtual memory** allows execution of processes not completely in memory.

| Feature     | Multiprogramming         | Timesharing               |
| ----------- | ------------------------ | ------------------------- |
| Goal        | Maximize CPU utilization | Minimize response time    |
| Interaction | None (batch)             | Interactive               |
| Scheduling  | Job scheduling           | CPU scheduling            |
| Memory      | Subset of jobs           | Swapping / virtual memory |
