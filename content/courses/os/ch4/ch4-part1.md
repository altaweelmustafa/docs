---
title: "Chapter 4 – Threads"
date: 2026-06-28
weight: 6
toc: true
tags:
  [
    "operating-systems",
    "threads",
    "multithreading",
    "kernel-threads",
    "user-threads",
    "thread-models",
  ]
description: "What a thread is, how it differs from a process, user-level vs kernel-level threads, and the three threading models."
author: "Mustafa Altaweel"
---

## Threads

### Process (Heavyweight Process)

A traditional **process** (also called a heavyweight process) contains:

**Shared among all threads in the process:**

- Code Section
- Data Section
- Opened Files

**Private to the process instance (PCB):**

- Process ID
- Register Set
- Program Counter (PC)
- Stack Section

```
┌──────────────────────────────┐
│  Code Section │ Data Section │ Open Files  │
├──────────────────────────────┤
│  PID │ Register Set │ Stack  │
│            PC →  ~~~~~       │
└──────────────────────────────┘
             Single-threaded Process
```

---

## Thread (Lightweight Process)

A **thread** is a lightweight process. It is the basic unit of CPU utilization.

### What a Thread Contains (Private to each thread)

- **Program Counter (PC)**
- **Register Set**
- **Stack Section**

### What All Peer Threads Share (within one process)

- **Code Section**
- **Data Section**
- **I/O Resources** (opened files)

```
┌───────────────────────────────────────────┐
│   Code Section │ Data Section │ Open Files │   ← Shared
├────────────────────┬──────────────────────┤
│  ID │ Reg │ Stack │  ID │ Reg │ Stack     │   ← Per thread
│    PC → ~~~~~     │    PC → ~~~~~         │
└────────────────────┴──────────────────────┘
              Multi-threaded Process (2 threads)
```

> **Advantage of threads**: **Sharing Resources** — threads within the same process share memory and files, so they can communicate without inter-process communication overhead.

---

## Two Kinds of Thread Support

### (1) User-Level Threads

- Thread management is entirely the **responsibility of the user** (the application).
- Very complex and difficult to implement correctly.
- The OS kernel is not aware of these threads.

### (2) Kernel-Level Threads

- **Most modern OSes support** this kind of threading.
- The kernel manages threads directly.
- The OS is aware of and schedules threads.

### Relationship Between User Threads and Kernel Threads

- Think of **user threads as processes (programs)**.
- Think of **kernel threads as CPUs**.

---

## Threading Models

### (1) Many-To-One

Many user threads are mapped to **one** kernel thread.

```
      (K)           ← 1 kernel thread
     / | \
    U  U  U         ← many user threads
```

> **Disadvantage**: No concurrent execution — if one user thread blocks, all block (since only one kernel thread).

---

### (2) One-To-One

Each **user thread** is assigned its own **kernel thread**.

```
(K)  (K)  (K)       ← kernel threads
 |    |    |
 U    U    U        ← user threads
```

> **Main advantage**: Allows **concurrent execution** — each thread runs independently.
>
> **Disadvantage**: We need enough kernel threads — creating too many can overwhelm the system.

---

### (3) Many-To-Many

Many user threads are mapped to an **equal or smaller number** of kernel threads.

```
(K)  (K)  (K)       ← kernel threads (≤ user threads)
  ╲  |  ╱
   U U U U U        ← user threads
```

> **Best of both worlds**: Allows concurrency (unlike Many-To-One) and doesn't require as many kernel threads as One-To-One.

---

## Summary Table

| Model            | User Threads | Kernel Threads | Concurrent? | Notes                        |
| ---------------- | ------------ | -------------- | ----------- | ---------------------------- |
| **Many-To-One**  | Many         | 1              | ✗ No        | Simple but blocks all on I/O |
| **One-To-One**   | N            | N              | ✓ Yes       | Needs many kernel threads    |
| **Many-To-Many** | Many         | ≤ Many         | ✓ Yes       | Most flexible, most complex  |
