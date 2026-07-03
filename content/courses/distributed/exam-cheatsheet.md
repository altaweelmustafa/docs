---
title: "Exam Cheatsheet"
date: 2026-07-03
description: "Quick revision page for the most important Distributed Systems definitions, formulas, and algorithm comparisons."
tags: [distributed-systems, exam, cheatsheet]
toc: true
weight: 99
---

## Chapter 1: Must-know definitions

| Term | Meaning |
|---|---|
| Distributed system | A collection of independent computers that appears to users as one coherent system. |
| Centralized | One main node controls or stores the service. |
| Decentralized | Control is spread, but the system may still have weak structure or partial interconnection. |
| Distributed | Components cooperate through a network and the system is designed around distribution. |
| Middleware | Software layer that hides distribution and provides common services such as communication, naming, security, replication, caching, and marshaling. |
| Transparency | Hiding distribution from users/applications. |
| Openness | Components can be reused, replaced, extended, or integrated with other systems. |
| Scalability | Ability to handle growth in size, geography, or administration without unacceptable performance or management problems. |

## Transparency types

| Type | Hides what? | Example |
|---|---|---|
| Access | Differences in data representation and access method | Local file vs remote file looks similar |
| Location | Where a resource is located | Use a name, not an IP address |
| Relocation | Resource can move while in use | Mobile object/server changes location |
| Migration | Resource may move to another location | VM moves to another host |
| Replication | Multiple copies exist | User sees one service, not many replicas |
| Concurrency | Many users share the same resource | Bank account updates are coordinated |
| Failure | Failure and recovery | System retries or switches replica |

## Dependability

| Concept | Meaning |
|---|---|
| Availability | Ready for use now. |
| Reliability | Continuous correct service over time. |
| Safety | Very low chance of catastrophic failure. |
| Maintainability | Easy to repair after failure. |
| Failure | Component does not meet its specification. |
| Error | Incorrect internal state that may cause failure. |
| Fault | Root cause of the error. |

**Metrics:**

- `MTTF`: average time until failure.
- `MTTR`: average time to repair.
- `MTBF = MTTF + MTTR`.

## Security triad

| Requirement | Meaning |
|---|---|
| Confidentiality | Only authorized parties can read data. |
| Integrity | Only authorized changes are allowed; tampering is detected/prevented. |
| Availability | Authorized users can access the system when needed. |

## Scalability formulas

For a centralized service modeled as a queue:

- Arrival rate: `λ`
- Service capacity: `μ`
- Service time: `S = 1/μ`
- Utilization: `U = λ/μ`
- Average number of requests in system: `N = U/(1-U)`
- Throughput: `X = λ`
- Response time: `R = N/X = S/(1-U)`
- Ratio: `R/S = 1/(1-U)`

**Important exam conclusion:** when `U → 1`, response time grows very fast. A service near 100% utilization becomes unusable.

## Chapter 3: Process and thread essentials

| Concept | Meaning |
|---|---|
| Processor | Physical execution engine for instructions. |
| Thread | Minimal software processor/execution context. |
| Process | Software processor containing one or more threads plus address space/resources. |
| Processor context | CPU registers such as PC, SP, addressing registers. |
| Thread context | Processor context plus thread state. |
| Process context | Thread context plus memory-management information such as MMU registers. |

**Why threads?** avoid blocking, exploit multicore parallelism, reduce process-switching overhead, and structure servers/clients more cleanly.

**Thread-level parallelism:**

`TLP = (Σ i·ci) / (1 - c0)`

where `ci` is the fraction of time exactly `i` threads execute simultaneously.

## Chapter 4: Communication essentials

| Type | Meaning |
|---|---|
| Transient communication | Message is discarded if it cannot be delivered immediately. |
| Persistent communication | Message is stored until it can be delivered. |
| Synchronous communication | Sender/client waits. |
| Asynchronous communication | Sender/client continues without waiting. |
| RPC | Remote Procedure Call hides message passing behind a procedure call. |
| MOM | Message-oriented middleware uses queues for persistent asynchronous messaging. |

**RPC steps:** client calls stub → stub marshals parameters → OS/network sends → server stub unmarshals → server executes → reply goes back through the reverse path.

**Why full access transparency is hard in RPC:** remote calls cannot behave exactly like local calls because parameters need marshaling, references are hard, failures are different, and communication takes time.

## Chapter 4: Epidemic protocol formulas

Anti-entropy:

- Pull: `p(i+1) = p(i)^2`
- Push: approximately `p(i+1) = p(i)e^-1` when `p(i)` is small and `N` is large.
- Push-pull reaches everyone in about `O(log N)` rounds.

Rumor spreading:

` s = e^(-(1/pstop + 1)(1-s)) `

Important: rumor spreading alone may leave some nodes ignorant; use anti-entropy or death certificates when correctness matters.

## Chapter 5: Clock essentials

| Concept | Meaning |
|---|---|
| Precision | Clocks of machines are close to each other. |
| Accuracy | A clock is close to real UTC time. |
| Internal synchronization | Keep clocks mutually precise. |
| External synchronization | Keep clocks accurate with respect to UTC. |
| Clock drift | Hardware clocks run slightly fast/slow. |

**Precision formula:** `∀t, ∀p,q: |Cp(t) - Cq(t)| ≤ π`  
**Accuracy formula:** `∀t, ∀p: |Cp(t) - t| ≤ α`

## Lamport logical clocks

Rules:

1. Before each local event, increment local counter.
2. When sending message, attach current counter as timestamp.
3. When receiving message with timestamp `ts(m)`, set local counter to `max(local, ts(m))`, then increment.
4. If timestamps tie, break ties using process IDs.

Guarantee:

- If `a → b`, then `C(a) < C(b)`.
- But `C(a) < C(b)` does **not** prove `a → b`.

## Vector clocks

`a` causally precedes `b` if:

- for all `k`: `ts(a)[k] ≤ ts(b)[k]`
- and for at least one `k`: `ts(a)[k] < ts(b)[k]`

If neither vector is less than the other, the events are concurrent/conflicting.

## Causal multicast delivery test

Message `m` from process `Pi` with timestamp `ts(m)` can be delivered at `Pj` when:

1. `ts(m)[i] = VCj[i] + 1`
2. For all `k ≠ i`: `ts(m)[k] ≤ VCj[k]`

## Mutual exclusion comparison

| Algorithm | Idea | Messages per entry/exit | Delay |
|---|---|---:|---:|
| Centralized | Ask coordinator | 3 | 2 message times |
| Ricart & Agrawala / distributed | Ask all other processes | `2(N-1)` | `2(N-1)` |
| Token ring | Token grants access | `1 ... ∞` | `0 ... N-1` |
| Decentralized voting | Need majority of replica coordinators | Larger, depends on attempts | Depends on attempts |

## Election algorithms

| Algorithm | Main idea | Best for |
|---|---|---|
| Bully | Higher-ID alive process wins. | Small/known groups. |
| Ring election | Election message circulates ring; highest priority wins. | Logical ring organization. |
| ZooKeeper leader election | Prefer server with most recent transaction, then higher ID. | Replicated coordination service. |
| Raft | Followers become candidates after timeout; majority vote elects leader. | Replicated logs/consensus. |
| Proof of stake | Random token owner becomes leader; more tokens means higher chance. | Blockchain-style systems. |

## Last-night exam checklist

- Explain transparency and give examples.
- Distinguish availability vs reliability.
- Solve simple utilization/response-time question.
- Compare cluster vs grid vs cloud/edge/pervasive.
- Explain threads vs processes and user-level vs kernel-level threads.
- Explain RPC and why parameter passing is difficult.
- Compare transient/persistent and sync/async communication.
- Explain anti-entropy, rumor spreading, and death certificates.
- Update Lamport clocks and vector clocks from a scenario.
- Decide if two vector timestamps are causally ordered or concurrent.
- Compare mutual-exclusion algorithms.
- Explain bully, ring, ZooKeeper, and Raft elections.
