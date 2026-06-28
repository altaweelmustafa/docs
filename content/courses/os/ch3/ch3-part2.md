---
title: "Chapter 3, Part 2 – Process Creation, Termination & Cooperating Processes"
date: 2026-06-28
weight: 5
toc: true
tags:
  [
    "operating-systems",
    "process-creation",
    "fork",
    "termination",
    "cooperating-processes",
    "producer-consumer",
    "buffer",
  ]
description: "How processes are created and terminated, the fork/exec model, cooperating vs. independent processes, and the Producer-Consumer problem with a circular buffer."
author: "Mustafa Altaweel"
---

## Process Creation

A **parent process** creates **children processes**, which in turn can create other processes — forming a **tree of processes**.

The root of the tree is the **INIT** process (in Unix/Linux).

```
        INIT
          |
        Parent
       /       \
    Child₁    Child₂
   / | \ \
  c  c  c  c
```

### Resource Sharing Options

When a parent creates a child, there are three resource-sharing possibilities:

1. **Parent and children share all** of the parent's resources.
2. **Children share a subset** of parent's resources.
3. **Parent and child share no resources** — they compete for all resources.

### Execution Options

- **Parent and children execute concurrently.**
- **Parent waits** until children terminate.

### Address Space

> **Address Space** = allocation of process in memory.

- **Child is a duplicate of parent** (same code and data).
- OR child has a **new program loaded into it**.

### Unix/Linux Example: `fork` and `exec`

- **`fork`** system call — creates a new process (child is a duplicate of parent).
- **`exec`** system call — used after a fork to **replace** the process's memory space with a new program.

**Example flow:**

```
fork()
  ↓
Computation runs concurrently on:
  CPU₀ (parent)    CPU₁ (child)
  (parallel execution)
```

---

## Process Termination

A process terminates when it executes its **last statement** and asks the OS to delete it (`exit`).

### Normal Termination

- Output data from child is passed to parent **via fork**.
- Process resources are **deallocated by OS**.
- When a process is killed, all its resources are deallocated.

### Abnormal Termination (Abort)

A **parent may terminate** the execution of its child process. Reasons:

1. Task assigned to child is **no longer required**.
2. Child has **exceeded its allocated resources**.
3. **Parent is exiting** — the OS doesn't allow the child to continue if its parent terminates.

> **Cascading Termination**: If the parent process ends (`exit`), all children and subchildren are also exited.

---

## Cooperating Processes

Concurrent processes are either:

1. **Independent process** — cannot affect or be affected by the execution of another process. Does not share data with others.
2. **Cooperating process** — can affect or be affected by the execution of another process. Shares data.

### Advantages of Process Cooperation

- **Information sharing** — multiple processes can access shared data.
- **Computation speed-up** — tasks can be split and run in parallel.
- **Modularity** — system is designed in modular fashion.
- **Convenience** — a user may work on several tasks simultaneously.

### Concurrency Requirements

Talking about concurrency requires:

- Cooperation among processes (communication among processes).
- **Synchronization** of process actions.

---

## Producer-Consumer Problem

The classic example of cooperating concurrent processes.

- **Producer process** — produces information (data).
- **Consumer process** — consumes this information.

```
Producer → [ Data / Buffer ] → Consumer
           (concurrently)
```

### Real-World Examples

- A **print program** produces characters consumed by the **printer**.
- A **compiler** produces assembly code consumed by the **assembler**.
- An **assembler** produces machine language code consumed by the **loader**.

### Data Structures Required

```c
const int n;          // size of buffer
type item;            // item = char, bit, word
var int buffer[n];
int in;               // index where we add items to the buffer
int out;              // index where we take items from the buffer
item nextP;           // next produced item
item nextC;           // next consumed item
```

A **circular buffer** is used in the implementation.

### Circular Buffer Conditions

- **Buffer is Full**: `(in + 1) % n == out`
- **Buffer is Empty**: `in == out`

### Buffer Diagram

```
[6]  E
[5]  D
[4]  C       → out = 2
[3]  B
[2]  A       → in = 1  (next slot to write)
[1] ///
[0]  F
    Buffer (n = 7)
```

### Producer Process

```
repeat
    produce an item in nextP;
    while ((in + 1) % n == out)   // busy waiting (buffer full)
        no-operation;
    buffer[in] = nextP;
    in = (in + 1) % n;
until false
```

### Consumer Process

```
repeat
    while (in == out)             // busy waiting (buffer empty)
        no-operation;
    nextC = buffer[out];
    out = (out + 1) % n;
    consume the item;
until false
```

> Producer and Consumer **run concurrently**.

### Disadvantage

We can only use **(n − 1) buffers** if we have **n** buffer slots. One slot is always kept empty to distinguish between full and empty states.
