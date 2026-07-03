---
title: "Chapter 4, Part 2 – Message-Oriented Communication"
date: 2026-07-03
description: "Sockets, ZeroMQ, MPI, queues, message brokers, and AMQP."
tags: [distributed-systems, messaging, sockets, zeromq, amqp, chapter4]
toc: true
weight: 2
---

## Transient messaging with sockets

Sockets provide low-level communication endpoints.

Berkeley socket operations:

| Operation | Meaning |
|---|---|
| `socket` | Create communication endpoint. |
| `bind` | Attach local address to socket. |
| `listen` | Set maximum number of pending connection requests. |
| `accept` | Block until connection request arrives. |
| `connect` | Actively establish connection. |
| `send` | Send data. |
| `receive` | Receive data. |
| `close` | Release connection. |

Important: sockets are powerful but low-level, so programming mistakes are common.

---

## Simple socket server/client idea

Server flow:

```text
create socket
bind to host/port
listen
accept connection
repeat:
    receive data
    send response
close
```

Client flow:

```text
create socket
connect to server
send request
receive response
close
```

---

## ZeroMQ: making sockets easier

ZeroMQ gives higher-level communication patterns using paired sockets.

All communication is asynchronous internally.

Three patterns in the slides:

| Pattern | Meaning | Example |
|---|---|---|
| Request-reply | Client sends request, server replies. | RPC-like communication. |
| Publish-subscribe | Publisher sends messages; subscribers receive matching topics. | News/time updates. |
| Pipeline | Producer pushes work; workers pull work. | Task distribution. |

---

## Request-reply pattern

Server uses reply socket (`REP`), client uses request socket (`REQ`).

Flow:

1. Server binds to address.
2. Client connects.
3. Client sends request.
4. Server receives and replies.
5. Client receives reply.

Good for simple request/response services.

---

## Publish-subscribe pattern

Publisher broadcasts messages with topics. Subscribers choose topics.

Example:

- publisher sends messages starting with `TIME`,
- subscriber subscribes to `TIME`,
- subscriber receives only matching messages.

Good for event/news dissemination.

---

## Pipeline pattern

Producer sends tasks to workers.

Flow:

1. Producer creates workload.
2. Producer pushes workload.
3. Workers pull tasks.
4. Workers execute tasks.

Good for load distribution.

---

## MPI operations

MPI gives flexible message-passing operations, often used in parallel/distributed computing.

| Operation | Meaning |
|---|---|
| `MPI_BSEND` | Append outgoing message to local send buffer. |
| `MPI_SEND` | Send and wait until copied to local or remote buffer. |
| `MPI_SSEND` | Send and wait until transmission starts. |
| `MPI_SENDRECV` | Send message and wait for reply. |
| `MPI_ISEND` | Start send and continue. |
| `MPI_ISSEND` | Start send and wait until receipt starts. |
| `MPI_RECV` | Receive; block if no message. |
| `MPI_IRECV` | Check/receive without blocking. |

---

## Queue-based messaging

Queue-based messaging supports persistent communication.

Instead of sender directly contacting receiver, sender puts message in a queue.

Core operations:

| Operation | Meaning |
|---|---|
| `PUT` | Append message to a queue. |
| `GET` | Block until queue has message, then remove first message. |
| `POLL` | Check queue and remove message if available; never block. |
| `NOTIFY` | Install handler called when message arrives. |

---

## Message-oriented middleware (MOM)

MOM provides asynchronous persistent communication using middleware-level queues.

Important idea:

- queues correspond to buffers at communication servers,
- applications put messages into local queues,
- queue managers route messages to other queues.

---

## Queue managers

Applications can only put into or get from local queues. Therefore, queue managers must route messages between queues.

This is why queue-based messaging needs routing infrastructure.

---

## Message brokers

Message queuing assumes applications share message format and protocol.

A message broker helps when applications are heterogeneous.

Broker roles:

- transform incoming messages to target format,
- act as application gateway,
- support subject-based routing / publish-subscribe.

---

## AMQP

AMQP was designed as a standard protocol for high-level messaging, similar in spirit to how TCP standardizes transport.

Basic model:

- client sets up stable connection,
- connection contains channels,
- two one-way channels can form a session,
- links are like sockets and maintain message transfer state.

AMQP producer/consumer examples in the slides show:

- declaring exchanges,
- declaring queues,
- binding queues to routing keys,
- publishing messages,
- consumers reading and acknowledging messages.

---

## Exam check

1. List socket operations and their meanings.
2. Compare request-reply, publish-subscribe, and pipeline.
3. What is the difference between `GET`, `POLL`, and `NOTIFY`?
4. Why do queue managers need routing?
5. What does a message broker do?
