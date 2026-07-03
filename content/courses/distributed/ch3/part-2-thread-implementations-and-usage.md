---
title: "Chapter 3, Part 2 – Thread Implementations and Distributed Use"
date: 2026-07-03
description: "User-level threads, kernel-level threads, two-level threading, clients, servers, and TLP."
tags: [distributed-systems, threads, clients, servers, chapter3]
toc: true
weight: 2
---

## Main issue

Should threads be implemented:

1. in user space, by a user-level thread library, or
2. inside the OS kernel?

Each choice has advantages and problems.

---

## User-level threads

User-level threads are managed by a library inside one process.

Advantages:

- thread operations are very fast,
- no kernel trap is needed for every thread operation,
- scheduling can be customized by the application.

Problems:

- the kernel may see only one process, not individual threads,
- if one user thread makes a blocking system call, the whole process may block,
- external events/signals are harder because the kernel cannot easily target a specific user thread.

Exam sentence:

> User-level threads are efficient, but blocking system calls can block the entire process.

---

## Kernel-level threads

Kernel-level threads are managed by the OS kernel.

Advantages:

- if one thread blocks, the kernel can schedule another thread in the same process,
- external events are easier to handle,
- good support for multiprocessor scheduling.

Problems:

- thread operations require system calls/traps to the kernel,
- historically less efficient than user-level thread operations.

Exam sentence:

> Kernel threads handle blocking better, but thread operations are more expensive.

---

## Two-level threading

Two-level threading combines user-level and kernel-level threads.

Basic idea:

- kernel threads execute user-level threads,
- if a user thread blocks in the kernel, the kernel can schedule another kernel thread,
- if a user thread blocks only at user level, the user-level package switches to another runnable user thread.

Principle:

1. A user thread does a system call.
2. The kernel thread executing it blocks.
3. The kernel may schedule another kernel thread that has a runnable user thread.
4. If no user thread is runnable, a kernel thread can remain idle or be destroyed.

---

## Threads on the client side

### Multithreaded web client

A browser uses threads to hide network latency.

Example:

1. Browser receives HTML.
2. Browser discovers images, CSS, scripts, and other files.
3. Each file can be fetched by a separate thread.
4. Browser displays files as they arrive.

### Multiple RPCs

A client may need results from several servers.

Instead of doing:

```text
call server A → wait
call server B → wait
call server C → wait
```

it can do:

```text
start thread for A
start thread for B
start thread for C
wait for all results
```

If calls go to different servers, the speedup can be close to linear.

---

## Thread-level parallelism (TLP)

Let `ci` be the fraction of time that exactly `i` threads are executing simultaneously.

```text
TLP = (Σ i·ci) / (1 - c0)
```

where `N` is the maximum number of threads that can execute at the same time.

Important slide observation:

Typical browsers have TLP around `1.5` to `2.5`, so threads in browsers are often more about organization and latency hiding than perfect parallelism.

---

## Threads on the server side

Servers use threads to improve performance and simplify structure.

Reasons:

- starting a thread is cheaper than starting a process,
- a single-threaded server cannot easily use multiple processors,
- one request can block while another request is handled,
- blocking I/O is easier to program than complex nonblocking logic.

---

## Dispatcher/worker model

Common server model:

1. Dispatcher receives incoming requests.
2. Dispatcher gives each request to a worker thread/process.
3. Worker handles the request.
4. Worker returns response or becomes available again.

Comparison:

| Model | Parallelism | Blocking calls |
|---|---|---|
| Multithreading | Yes | Yes, simple to use. |
| Single-threaded process | No | Yes, but blocks the whole server. |
| Finite-state machine | Yes | Uses nonblocking calls; harder to program. |

---

## Exam check

1. Compare user-level and kernel-level threads.
2. Why are kernel threads better for blocking system calls?
3. Explain two-level threading.
4. How do threads hide latency in a browser?
5. What is the dispatcher/worker model?
