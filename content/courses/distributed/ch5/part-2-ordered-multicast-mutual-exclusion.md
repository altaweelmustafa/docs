---
title: "Chapter 5, Part 2 – Ordered Multicast and Mutual Exclusion"
date: 2026-07-03
description: "Totally ordered multicast, Lamport mutual exclusion, centralized, Ricart–Agrawala, token ring, decentralized voting, and ZooKeeper locks."
tags: [distributed-systems, mutual-exclusion, ordered-multicast, zookeeper, chapter5]
toc: true
weight: 2
---

## Totally ordered multicast

Problem:

Concurrent updates on replicated data must be seen in the same order everywhere.

Example from slides:

- Initial account: `$1000`.
- `P1` adds `$100`.
- `P2` increments by `1%`.

If replicas apply updates in different orders:

- add then 1% → `$1111`,
- 1% then add → `$1110`.

So all replicas must apply updates in the same order.

---

## Totally ordered multicast solution

Each process keeps a local queue ordered by timestamp.

Rules:

1. Process `Pi` sends timestamped message `mi` to all processes.
2. `Pi` puts `mi` in its own queue.
3. Any incoming message at `Pj` is inserted in `queuej` according to timestamp.
4. Incoming messages are acknowledged to every other process.

A process `Pj` delivers message `mi` only if:

1. `mi` is at the head of `queuej`, and
2. for every process `Pk`, there is a message in `queuej` with a larger timestamp.

Assumption:

- communication is reliable,
- communication is FIFO ordered.

---

## Lamport clocks for mutual exclusion

Mutual exclusion is about agreeing on the order in which processes enter a critical region.

Analogy:

- totally ordered multicast: all processes build the same message order,
- mutual exclusion: all processes agree on the same request order.

Lamport-style mutual exclusion uses:

- timestamped `ENTER` request,
- acknowledgments,
- `RELEASE` messages,
- sorted local queues.

A process may enter when:

1. its own request is at the head of the queue,
2. it has heard from all other processes.

---

## Mutual exclusion problem

Several processes want exclusive access to a shared resource.

Basic solution categories:

| Category | Meaning |
|---|---|
| Permission-based | Process asks one or more processes for permission. |
| Token-based | Process can enter only when it holds the token. |

---

## Centralized permission-based mutual exclusion

A coordinator grants access.

Flow:

1. `P1` asks coordinator for permission.
2. Coordinator grants permission.
3. `P2` asks while `P1` is using resource.
4. Coordinator delays reply to `P2`.
5. `P1` releases resource.
6. Coordinator grants permission to `P2`.

Messages per entry/exit: `3`

Delay before entry: `2` message times.

Advantages:

- simple,
- efficient message count.

Disadvantages:

- coordinator bottleneck,
- coordinator is single point of failure.

---

## Ricart & Agrawala distributed mutual exclusion

This is like Lamport mutual exclusion but with deferred acknowledgments.

A process sends request to all other processes.

A receiver replies only if:

1. it has no interest in the resource, or
2. it is waiting but has lower priority than requester.

Priority is decided by timestamp, with process ID as tie breaker.

If receiver has higher priority, it defers the reply until after it exits.

Messages per entry/exit:

```text
2(N - 1)
```

Delay:

```text
2(N - 1)
```

Advantages:

- no central coordinator,
- fully distributed decision.

Disadvantages:

- many messages,
- every process must participate,
- failure of a process can block progress unless failure handling exists.

---

## Token ring algorithm

Processes are organized in a logical ring. A token circulates.

Rule:

- process holding token may enter critical region,
- if it does not need the resource, it passes token on.

Messages per entry/exit: `1 ... ∞`

Delay: `0 ... N-1` message times.

Advantages:

- no permission requests to all processes,
- fair if token circulates correctly.

Disadvantages:

- token loss is serious,
- process failure can break ring,
- waiting time depends on token position.

---

## Decentralized voting mutual exclusion

Assume every resource is replicated `N` times. Each replica has its own coordinator.

To access resource:

- a process must get majority vote from `m > N/2` coordinators,
- coordinators respond immediately.

Assumption:

A coordinator that crashes recovers quickly but forgets permissions it granted.

Correctness violation can happen if enough coordinators reset and grant conflicting permissions.

Definitions:

```text
p = Δt / T
```

where `p` is probability a coordinator resets during interval `Δt`, and `T` is lifetime.

Probability that `k` out of `m` reset:

```text
P[k] = C(m,k) p^k (1-p)^(m-k)
```

Violation happens when:

```text
f ≥ 2m - N
```

Violation probability:

```text
Σ from k=2m-N to m of P[k]
```

Exam idea:

- larger majority can reduce violation probability,
- but larger majority increases communication cost and may hurt availability.

---

## Mutual exclusion comparison table

| Algorithm | Messages per entry/exit | Delay before entry | Main weakness |
|---|---:|---:|---|
| Centralized | `3` | `2` | Coordinator bottleneck/failure. |
| Distributed / Ricart & Agrawala | `2(N-1)` | `2(N-1)` | High message cost; process failures. |
| Token ring | `1 ... ∞` | `0 ... N-1` | Lost token or broken ring. |
| Decentralized | `2kN + (k-1)N/2 + N` | `2kN + (k-1)N/2` | More complex, majority/failure tradeoff. |

---

## ZooKeeper locking

ZooKeeper provides a centralized coordination service with a tree-like namespace.

Basics:

- clients can create/delete/update/check nodes,
- communication is nonblocking,
- clients can subscribe to change notifications.

### Race condition

Simple lock idea:

1. `C1` creates `/lock`.
2. `C2` wants lock but sees `/lock` exists.
3. Before `C2` subscribes to notification, `C1` deletes `/lock`.
4. `C2` subscribes and waits forever because it missed the change.

Solution: use version numbers and notification protocol carefully.

### Version notation

| Notation | Meaning |
|---|---|
| `W(n,k)a` | Write value `a` to node `n` only if current version is `k`. |
| `R(n,k)` | Current version of node `n` is `k`. |
| `R(n)` | Client asks current value of node `n`. |
| `R(n,k)a` | Returned value `a` with version `k`. |

### Locking protocol

1. Client creates `/lock`.
2. If `/lock` exists, another client subscribes to changes.
3. Owner deletes `/lock` on unlock.
4. Subscribers are notified and try again.

---

## Exam check

1. Why do replicated database updates need total order?
2. What are the delivery conditions in totally ordered multicast?
3. Compare permission-based and token-based mutual exclusion.
4. Explain Ricart & Agrawala.
5. Compare centralized, distributed, token ring, and decentralized algorithms.
6. What race condition can happen in simple ZooKeeper locking?
