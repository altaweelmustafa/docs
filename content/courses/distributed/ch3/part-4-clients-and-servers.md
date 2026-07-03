---
title: "Chapter 3, Part 4 – Clients and Servers"
date: 2026-07-03
description: "Client-side software, networked user interfaces, server organization, state, and object servers."
tags: [distributed-systems, clients, servers, chapter3]
toc: true
weight: 4
---

## Client-server interaction

A client requests a service; a server provides it.

In distributed systems, we distinguish:

- application-level solutions,
- middleware-level solutions.

Middleware often helps hide distribution by providing stubs, naming, communication, and failure handling.

---

## Networked user interfaces

### X Window system idea

The X Window example shows that “client” and “server” can be confusing.

- The application acts as an X client.
- The X kernel/server runs on the user’s machine and controls display/input.

So the machine in front of the user can run the X server, while the application can run remotely.

### Practical problems with X

- application logic and UI commands are often mixed,
- applications may communicate synchronously with the X kernel,
- synchronous interaction over network can be slow.

Alternatives:

- send low-level pixel updates, such as VNC-style remote desktop,
- provide high-level display operations optimized locally.

---

## Virtual desktop environment

With cloud applications, the question becomes: how can users access remote applications seamlessly?

Common answer:

- use a web browser as the ultimate networked user interface,
- application logic may run in cloud,
- browser provides local rendering and interaction.

---

## Client-side software for transparency

Client-side software often implements distribution transparency.

| Transparency | Client-side support |
|---|---|
| Access transparency | Client stubs hide RPC/message details. |
| Location/migration transparency | Client software tracks actual location. |
| Replication transparency | Client stub invokes multiple replicas or chooses one. |
| Failure transparency | Client retries, masks server/communication failures when possible. |

Important exam point:

> Many forms of transparency are implemented partly or fully on the client side.

---

## Server general organization

A server is a process that implements a service for clients.

Basic cycle:

1. wait for request,
2. receive request,
3. handle request,
4. send response,
5. wait for next request.

---

## Iterative vs concurrent servers

| Server type | Meaning | Problem/advantage |
|---|---|---|
| Iterative server | Handles one request completely before accepting the next. | Simple but poor under blocking/long requests. |
| Concurrent server | Dispatcher accepts requests and passes them to threads/processes. | Handles many requests and blocking operations better. |

Concurrent servers are the norm because real servers often wait for disk, network, database, or other services.

---

## Contacting a server

Many services are tied to well-known ports.

Examples:

| Service | Port |
|---|---:|
| FTP data | 20 |
| FTP control | 21 |
| Telnet | 23 |
| SMTP | 25 |
| HTTP | 80 |

If a service endpoint is assigned dynamically, clients need a way to discover it, often through a naming or directory service.

---

## Out-of-band communication

Problem:

> Can we interrupt a server while it is handling a request?

Solutions:

1. Use a separate port for urgent data.
   - Server has a separate thread/process for urgent messages.
   - Urgent message can pause the current request.
   - Needs priority scheduling support.
2. Use transport-layer urgent-data facilities.
   - Example: TCP urgent messages.
   - OS signaling can catch urgent messages.

---

## Stateless servers

A stateless server does not keep accurate client state after handling a request.

It does not:

- remember whether a client opened a file,
- promise to invalidate client caches,
- keep track of clients.

Advantages:

- clients and servers are more independent,
- crashes cause fewer state inconsistencies,
- easier recovery.

Disadvantage:

- lower performance because the server cannot predict client behavior, e.g., cannot prefetch based on open-file state.

Exam question: does connection-oriented communication fit stateless design?

Answer: not naturally. A connection usually creates state, but a server can still design application logic to minimize client state.

---

## Stateful servers

A stateful server keeps track of clients.

Examples:

- records that a file is open,
- knows what data the client cached,
- allows clients to keep local copies.

Advantages:

- can improve performance through caching and prefetching,
- can support richer behavior.

Disadvantages:

- client/server crashes can create inconsistent state,
- recovery is harder,
- server must clean stale state.

---

## Object servers

An object server receives invocation requests for objects.

Important questions:

- Where are code and data of the object?
- Which object should be activated?
- Which threading model should be used?
- Should modified state be kept?

Object adapter:

- implements activation policy,
- connects incoming invocations to object implementation.

The Ice runtime example in the slides shows a server registering objects and a client invoking them through proxies.

---

## Exam check

1. What is the strange client/server naming in X Window?
2. How does client-side software support transparency?
3. Compare iterative and concurrent servers.
4. Explain out-of-band communication.
5. Compare stateless and stateful servers.
