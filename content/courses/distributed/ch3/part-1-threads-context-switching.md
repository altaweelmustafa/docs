---
title: "Chapter 3, Part 1 – Threads and Context Switching"
date: 2026-07-03
description: "Processors, threads, processes, contexts, context switching costs, and why threads are used."
tags: [distributed-systems, threads, processes, chapter3]
toc: true
weight: 1
---

## Basic idea

Distributed systems run many activities at the same time. To organize those activities, we use processes and threads.

The slides describe threads as **virtual processors in software** built on top of physical processors.

---

## Processor, thread, and process

| Concept | Meaning |
|---|---|
| Processor | Physical execution unit that provides instructions and executes instruction sequences. |
| Thread | Minimal software processor/execution context in which instructions can execute. |
| Process | Software processor/context in which one or more threads can execute. |

Simple way:

- processor = real hardware worker,
- thread = one line of execution,
- process = container for one or more threads plus resources/address space.

---

## Contexts

A context is the information needed to stop execution and continue later.

| Context type | Contains |
|---|---|
| Processor context | Minimal CPU register values such as program counter, stack pointer, addressing registers. |
| Thread context | Processor context plus thread state stored in registers and memory. |
| Process context | Thread context plus process-specific memory-management data, such as MMU registers. |

### Why process switching is more expensive

Thread switching can often stay inside the same address space. Process switching usually involves OS/kernel work and memory-management changes.

Important observations:

1. Threads share the same address space.
2. Thread context switching may be independent of the OS in user-level implementations.
3. Process switching is usually more expensive because it involves the kernel.
4. Creating and destroying threads is cheaper than creating/destroying processes.

---

## Why use threads?

| Reason | Explanation | Distributed-system example |
|---|---|---|
| Avoid needless blocking | If one thread blocks on I/O, another thread can run. | Web server handles another request while one request waits for disk/network. |
| Exploit parallelism | Multiple threads can run on multicore processors. | Server uses several cores for requests. |
| Avoid process switching | Threads are cheaper than processes. | Large application uses worker threads instead of separate processes. |
| Better structure | Blocking calls make code easier to write than complex nonblocking state machines. | Thread per request or worker pool. |

---

## Tradeoffs of threads

Threads are useful but risky.

Advantages:

- cheaper than processes,
- good for blocking I/O,
- can exploit multiple cores,
- simplify server/client organization.

Disadvantages:

- share the same address space,
- bugs in one thread can corrupt memory used by another thread,
- synchronization is needed to protect shared data,
- race conditions and deadlocks become possible.

---

## Cost of a context switch

Context switching has two cost types:

| Cost type | Meaning |
|---|---|
| Direct cost | Time to save/restore context and run scheduler/interrupt handler. |
| Indirect cost | Performance loss caused by cache/TLB disruption. |

The slide diagram shows that after a context switch, useful cached blocks may be replaced. Later, when the old thread resumes, it may need to reload data. This hidden cache cost can be larger than the visible switch cost.

---

## Processes vs threads in the Python example

The slides show a Python example using `multiprocessing.Process` and `threading.Thread`.

Main lesson:

- separate processes do not share the same memory by default,
- threads inside the same process share variables,
- when multiple threads update shared variables, the order of updates depends on scheduling.

Exam point:

> Shared address space is both the advantage and danger of threads.

---

## Exam check

1. Define processor, thread, and process.
2. What information is stored in a process context but not necessarily in a thread context?
3. Why is process switching usually more expensive than thread switching?
4. What are direct and indirect context-switch costs?
5. Why can shared address space be dangerous?
