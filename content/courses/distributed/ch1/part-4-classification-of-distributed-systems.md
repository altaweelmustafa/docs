---
title: "Chapter 1, Part 4 – Classification of Distributed Systems"
date: 2026-07-03
description: "High-performance, information, and pervasive distributed systems."
tags: [distributed-systems, classification, chapter1]
toc: true
weight: 4
---

## Why classify distributed systems?

Distributed systems come in many forms. The chapter groups them into broad categories so you know what problem each type solves.

Main categories:

1. High-performance distributed computing.
2. Distributed information systems.
3. Distributed pervasive systems.

---

## High-performance distributed computing

This category focuses on using many machines/processors to compute faster or handle large workloads.

### Parallel computing

Parallel computing started as the high-performance direction of distributed computing.

Important distinction:

| Term | Meaning |
|---|---|
| Multiprocessor / multicore | Multiple processors/cores share memory tightly. |
| Multicomputer | Multiple computers connected by a network. |

Multiprocessors are easier to program because memory sharing is natural. Multicomputers scale better physically but are harder to program.

---

## Distributed Shared Memory (DSM)

DSM tries to make a multicomputer look like it has shared memory.

Basic idea:

- Each processor has its own memory.
- The system creates one virtual address space.
- If processor A accesses a page stored at processor B, the OS fetches the page from B.

Why it sounded good:

- programmers can use a shared-memory model,
- easier than message passing.

Why it failed in practice:

- performance could not compete with real multiprocessors,
- page movement and synchronization were expensive,
- expectations were too high.

---

## Cluster computing

A cluster is a group of high-end systems connected through a LAN.

Typical properties:

- homogeneous hardware/software,
- same OS or near-identical OS,
- tightly coupled management node(s),
- often used for high performance or high availability.

Exam contrast:

> Cluster = many similar machines in one local environment.

---

## Grid computing

A grid connects many nodes from different places and organizations.

Typical properties:

- heterogeneous machines,
- spread across several organizations,
- can span a WAN,
- resource sharing is done through virtual organizations.

A virtual organization is a group of users/IDs allowed to use shared resources across real organizations.

Exam contrast:

> Grid = heterogeneous resources across organizations.

---

## Grid architecture layers

| Layer | Responsibility |
|---|---|
| Fabric | Interfaces to local resources: state, capabilities, locking. |
| Connectivity | Communication, transaction, and authentication protocols. |
| Resource | Manages one resource: create process, read data, control local access. |
| Collective | Manages multiple resources: discovery, scheduling, replication. |
| Application | Actual grid applications. |

---

## Distributed information systems

These systems focus on integrating information and applications.

Original problem:

Organizations had many networked applications, but interoperability was difficult.

Basic integration approach:

1. A client sends requests to different applications.
2. The client collects results.
3. The client presents one coherent result to the user.

Next step: direct application-to-application communication, called Enterprise Application Integration (EAI).

---

## Transactions and ACID

A transaction is a sequence of operations that should behave as one unit.

Common primitives:

| Primitive | Meaning |
|---|---|
| `BEGIN TRANSACTION` | Start transaction. |
| `END TRANSACTION` | Try to commit transaction. |
| `ABORT TRANSACTION` | Cancel and restore old values. |
| `READ` | Read data. |
| `WRITE` | Write data. |

ACID properties:

| Property | Meaning |
|---|---|
| Atomicity | All-or-nothing; transaction appears indivisible. |
| Consistency | System invariants remain valid. |
| Isolation | Concurrent transactions do not interfere incorrectly. |
| Durability | Once committed, changes are permanent. |

---

## Transaction Processing Monitor (TP Monitor)

When transaction data is spread across multiple servers, a TP monitor coordinates the transaction.

Its job is to make distributed transaction execution look like one reliable transaction.

---

## Middleware and EAI

Middleware supports integration by providing communication facilities.

| Middleware style | Meaning |
|---|---|
| RPC | Request is written like a local procedure call, but sent as messages to another machine. |
| MOM | Message-oriented middleware sends messages to logical contact points/queues and forwards to subscribers. |

---

## How applications can be integrated

| Method | Advantage | Problem |
|---|---|---|
| File transfer | Simple. | Not flexible; must manage file formats, layout, update propagation. |
| Shared database | More flexible. | Needs common schema and can become bottleneck. |
| RPC | Good when one application needs to execute operations in another. | Caller and callee usually must be active at same time. |
| Messaging | Decouples sender and receiver in time and space. | Requires message infrastructure and routing. |

---

## Exam check

1. Compare cluster and grid computing.
2. Why did DSM not become very successful?
3. What are the layers of grid architecture?
4. What does ACID mean?
5. Compare file transfer, shared database, RPC, and messaging for integration.
