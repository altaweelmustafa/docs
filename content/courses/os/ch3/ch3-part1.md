---
title: "Chapter 3, Part 1 – Process Concept, States, PCB & Scheduling"
date: 2026-06-28
weight: 4
toc: true
tags:
  [
    "operating-systems",
    "process",
    "PCB",
    "process-states",
    "scheduling",
    "context-switch",
  ]
description: "What a process is, its memory layout, the five process states and their transitions, the Process Control Block, and the three scheduling levels."
author: "Mustafa Altaweel"
---

## Process Concept

> **Definition**: A **process** is a program in execution.
> `process ≡ program ≡ job`

A **program** is a passive entity (a file on disk). A **process** is an active entity — a program loaded into memory and executing.

### Types of Processes in the System

1. **Batch Process** — generally has low priority.
2. **Time Sharing Process** — interactive; includes users, program development, data entry, gaming.
3. **System Tasks** — e.g. interrupt handling.

---

## Process Contents (Memory Layout)

A process in memory contains three sections:

1. **Code Section (Program Counter "PC")**
   - Holds the instructions of the program.
   - **PC** = the address of the instruction currently being executed.

2. **Data Section**
   - Memory allocation where input and output data is stored.
   - Global variables (static).

3. **Stack Section**
   - Used for function calls, local variables, return addresses.

```
┌─────────────────┐
│   Code Section  │  ← PC points here
│      "PC"       │
├─────────────────┤
│   Stack Section │  ↕ grows/shrinks
├─────────────────┤
│   Data Section  │
└─────────────────┘
        Process
```

---

## Process States

A process passes through **5 states** during its lifetime:

| State          | Description                                                                                           |
| -------------- | ----------------------------------------------------------------------------------------------------- |
| **NEW**        | The process has been newly created. It enters the **Job Queue (Pool)**.                               |
| **READY**      | The process is loaded into memory and ready to run; it is in the **Ready Queue** waiting for the CPU. |
| **RUNNING**    | The process has the CPU and is executing.                                                             |
| **WAITING**    | The process is waiting for some event to happen — typically waiting for I/O completion.               |
| **TERMINATED** | The process has finished execution.                                                                   |

### State Transition Diagram

```
         CPU Scheduling              Exit
  NEW → READY ──────────→ RUNNING ────────→ TERMINATED
          ↑    ← Interrupt ←  |
          |                   | I/O needed
          |    I/O Completed  ↓
          └──────────── WAITING
```

---

## Process Control Block (PCB)

**Where is data about the process stored?**

→ In the **Process Control Block (PCB)** — a data structure (essentially a table) that the OS maintains for every process.

### PCB Contains

| Field                    | Description                                              |
| ------------------------ | -------------------------------------------------------- |
| **Process ID**           | Unique identifier for the process                        |
| **Process State**        | Current state (New, Ready, Running, Waiting, Terminated) |
| **Program Counter (PC)** | Address of the next instruction to execute               |
| **Registers**            | Snapshot of all CPU registers                            |
| **Scheduling Info**      | Priority, scheduling queue pointers                      |
| **Memory Info**          | Base and limit registers                                 |
| **Accounting Info**      | CPU time used, time limits, job IDs                      |

---

## Scheduling & System Queues

### Queue Types

```
Job Queue (Pool)
     ↓ [Long-Term / Job Scheduling]
Ready Queue ──[Short-Term / CPU Scheduling]──→ CPU → Exit
     ↑  ↑                                      |
     |  └──── "queue is finished" (Interrupt) ←┘
     |                                          |
     └── I/O Completed ← I/O Queue ←── I/O required
         "HD Scheduling Algorithm"
```

There is also an **Intermediate Queue** fed by **Medium-Term Scheduling**.

---

## Three Levels of Scheduling

### Long-Term Scheduling ("Job Scheduling")

- Selects a job from the **Job Queue (pool)** to be admitted into memory.
- Once admitted, the job is added to the **Ready Queue**.
- Invoked infrequently (seconds, minutes) → the OS has **enough time** to decide carefully which job to fetch.
- **Controls the degree of multiprogramming** (the number of jobs in the Ready Queue / memory).

### Short-Term Scheduling ("CPU Scheduling")

- Selects a process from the **Ready Queue** to be given the CPU to **run**.
- Invoked very frequently (milliseconds, microseconds, nanoseconds) → must be **very fast**.

### Job Scheduling Strategy: Mix of CPU & I/O Bound Jobs

- If most jobs in memory are **CPU-bound**: CPU is always busy but I/O queues are **empty** → unbalanced.
- If most jobs in memory are **I/O-bound**: I/O queues are always full but CPU is almost **free** → unbalanced.
- **Long-Term Scheduling** selects a **mix of CPU-bound and I/O-bound jobs** so the system will be **reasonably balanced**.

> **Degree of Multiprogramming** = the number of jobs in memory (Ready Queue). Long-Term Scheduling controls this degree.
