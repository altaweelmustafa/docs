---
title: "Chapter 4, Part 1 – Foundations and RPC"
date: 2026-07-03
description: "Layered communication, transient/persistent, synchronous/asynchronous, client-server communication, and RPC."
tags: [distributed-systems, communication, rpc, chapter4]
toc: true
weight: 1
---

## Basic networking model

Traditional networking is layered. The slides mention that this model has drawbacks for distributed systems:

- focuses mainly on message passing,
- includes functionality that may be unnecessary,
- can violate access transparency.

---

## Low-level layers recap

| Layer | Role |
|---|---|
| Physical | Specification and transmission of bits. |
| Data link | Groups bits into frames; handles error/flow control. |
| Network | Routes packets through a network. |

For many distributed systems, the lowest-level useful interface is the network layer.

---

## Transport layer

The transport layer provides actual communication facilities for most distributed systems.

| Protocol | Meaning |
|---|---|
| TCP | Connection-oriented, reliable, stream-oriented communication. |
| UDP | Unreliable best-effort datagram communication. |

---

## Middleware layer

Middleware exists to provide common services and protocols for many applications.

Examples of middleware services:

- communication protocols,
- marshaling/unmarshaling data,
- naming protocols,
- security protocols,
- scaling mechanisms such as replication and caching.

After middleware handles general distributed-system problems, the application only needs application-specific protocols.

---

## Types of communication

Two important dimensions:

| Dimension | Type | Meaning |
|---|---|---|
| Durability | Transient | Message is discarded if it cannot be delivered immediately. |
| Durability | Persistent | Message is stored until it can be delivered. |
| Synchronization | Asynchronous | Sender continues without waiting. |
| Synchronization | Synchronous | Sender waits at some point. |

---

## Places for synchronization

A sender can synchronize at different points:

1. **At request submission:** wait until the system accepts the request.
2. **At request delivery:** wait until request reaches receiver.
3. **After request processing:** wait until receiver processes and replies.

The more the sender waits, the more synchronous the communication feels.

---

## Client-server communication

Classic client/server is usually:

- transient,
- synchronous,
- request/reply.

Properties:

- client and server must be active at communication time,
- client sends request and blocks until reply,
- server waits for requests and processes them.

Drawbacks:

- client cannot do other work while waiting,
- failures must be handled immediately,
- not suitable for systems like email/news where receiver may be offline.

---

## Messaging

Message-oriented middleware targets persistent asynchronous communication.

Properties:

- processes send messages to queues,
- sender does not need immediate reply,
- middleware can provide fault tolerance,
- sender and receiver can be decoupled in time.

---

## Remote Procedure Call (RPC)

RPC tries to make remote communication look like a local procedure call.

Why RPC is attractive:

- programmers understand procedure calls,
- procedures can be treated as black boxes,
- in principle, a procedure can run on another machine.

Main idea:

> Hide communication between caller and callee behind the procedure-call mechanism.

---

## Basic RPC operation

Steps:

1. Client calls a client stub.
2. Client stub builds a message and asks local OS to send it.
3. Client OS sends message to remote OS.
4. Server OS gives message to server stub.
5. Server stub unpacks parameters and calls server procedure.
6. Server procedure executes and returns result to server stub.
7. Server stub builds reply message.
8. Reply is sent back through OS/network.
9. Client stub receives reply.
10. Client stub unpacks result and returns to client code.

---

## Parameter passing in RPC

RPC is harder than just wrapping parameters in a message.

Problems:

- machines may use different byte ordering,
- integers/floats/characters may be represented differently,
- arrays/records/unions need agreed encoding,
- pointers/references cannot be passed like local references,
- failures are different from local calls.

### Marshaling and unmarshaling

| Term | Meaning |
|---|---|
| Marshaling | Convert parameters/results into byte sequence for transmission. |
| Unmarshaling | Convert byte sequence back into machine-specific representation. |

---

## Copy in/copy out semantics

Assumption:

- procedure receives copies of parameters,
- during execution, nothing can be assumed about original parameter values,
- all data operated on should be passed as parameters.

This excludes normal local references to global memory.

Conclusion:

> Full access transparency cannot be completely achieved with RPC.

Remote references can improve transparency by allowing remote objects/data to be accessed through reference-like handles.

---

## Asynchronous RPC

Asynchronous RPC tries to remove strict request-reply blocking.

Basic idea:

- client sends request,
- client continues without waiting,
- reply may be ignored, polled, or handled later.

This helps hide latency and improve concurrency.

---

## Multiple RPCs

A client may send one RPC request to a group of servers.

Use cases:

- parallel search,
- replicated service calls,
- collecting results from multiple services,
- improving availability by contacting several replicas.

---

## Exam check

1. Compare TCP and UDP.
2. What services does middleware provide?
3. Compare transient and persistent communication.
4. Why is classic client-server synchronous?
5. Explain the 10-step RPC flow.
6. Why is full access transparency hard in RPC?
