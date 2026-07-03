---
title: "Chapter 1, Part 2 – Design Goals, Transparency, and Openness"
date: 2026-07-03
description: "Resource sharing, transparency types, limits of transparency, openness, policies, and mechanisms."
tags: [distributed-systems, design-goals, transparency, openness, chapter1]
toc: true
weight: 2
---

## Overall design goals

Distributed systems are usually designed to achieve four main goals:

| Goal | Meaning |
|---|---|
| Resource sharing | Allow users/applications to share files, storage, printers, services, data, computation, etc. |
| Distribution transparency | Hide the fact that components are distributed. |
| Openness | Make the system easy to extend, integrate, and reuse. |
| Scalability | Keep working when size, distance, or administration grows. |

---

## Resource sharing

Resource sharing means making resources available to users and applications across the network.

Examples:

- Cloud storage and shared files.
- Peer-to-peer multimedia streaming.
- Shared mail services.
- Shared web hosting and content distribution networks.

Important idea: sharing is not enough. Once resources are shared, we also need security, concurrency control, naming, fault tolerance, and performance management.

---

## Distribution transparency

Transparency means the system hides distribution details from users and programmers.

The user should not need to know:

- where the resource is,
- how many copies exist,
- whether another user is accessing it,
- whether a failure happened and recovery occurred.

### Types of transparency

| Transparency type | What it hides | Example |
|---|---|---|
| Access | Differences in data representation and access method. | Remote file access looks like local file access. |
| Location | Where an object/resource is located. | Use `www.example.com` instead of knowing the server IP. |
| Relocation | Object can move while in use. | A service moves to another server during execution. |
| Migration | Object can move to another location. | VM or mobile code moves to a new host. |
| Replication | Multiple copies exist. | User sees one database, even if many replicas exist. |
| Concurrency | Resource is shared by independent users. | Two users update shared data safely. |
| Failure | Failure and recovery of an object. | Request is retried on another server. |

### Important difference: relocation vs migration

- **Migration transparency:** hides that something moved.
- **Relocation transparency:** hides that something moved while it was being used.

Relocation is stronger.

---

## Full transparency is not always good

The slides warn that aiming for complete transparency may be too much.

Why?

1. **Latency cannot be fully hidden.** Remote communication takes time.
2. **Failures cannot be perfectly hidden.** A slow machine and a failed machine can look the same for a while.
3. **Sometimes users need to know distribution exists.** Example: location-based services, time zones, or reporting that a remote server is not responding.

Exam sentence:

> Transparency is useful, but complete transparency can be impossible or even harmful because latency, failures, and location-specific behavior may need to be exposed.

---

## Openness

An open distributed system offers components that can easily be used by or integrated into other systems.

Openness usually requires:

- clear interfaces,
- standard protocols,
- separation between implementation and interface,
- ability to replace or extend components.

### Policies vs mechanisms

| Concept | Meaning | Example |
|---|---|---|
| Policy | What decision should be made? | What consistency level do we require? |
| Mechanism | How do we make that decision possible? | Replication protocol or cache invalidation mechanism. |

Examples of policies:

- What consistency do we require for cached data?
- Which operations can downloaded code perform?
- What quality of service is needed under low bandwidth?
- What secrecy level is needed for communication?

### Why strict separation can be hard

Separating policy from mechanism sounds good, but if the separation is too strict, the system may need many configuration options and become hard to manage.

So the goal is balance: flexible enough to be open, but not so configurable that it becomes impossible to operate.

---

## Exam check

1. Define distribution transparency.
2. List and explain the seven transparency types.
3. Why is full transparency sometimes impossible?
4. What is the difference between policy and mechanism?
