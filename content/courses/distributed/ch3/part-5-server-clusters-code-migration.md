---
title: "Chapter 3, Part 5 – Server Clusters and Code Migration"
date: 2026-07-03
description: "Server clusters, request dispatching, CDNs, reasons for code migration, weak/strong mobility, and VM migration."
tags: [distributed-systems, server-clusters, cdn, code-migration, chapter3]
toc: true
weight: 5
---

## Server clusters

A server cluster is a group of servers working together to provide one service.

Common three-tier organization:

1. **First tier:** request dispatching / front-end.
2. **Second tier:** application or service logic.
3. **Third tier:** storage/database/backend resources.

Crucial element: the first tier passes incoming requests to the correct server.

---

## Request dispatching bottleneck

If the first tier handles all communication to/from the cluster, it can become a bottleneck.

Solution idea: **TCP handoff**.

Basic idea:

1. Front-end receives request.
2. Front-end chooses a back-end server.
3. TCP connection is handed off or redirected so back-end can handle communication.

This reduces pressure on the front-end.

---

## Servers spread across the Internet

Spreading servers across the Internet improves locality but creates administrative problems.

Common solution:

- use data centers from a single cloud provider,
- use DNS-based request dispatching.

### DNS-based dispatching

1. Client looks up service name through DNS.
2. Client IP is part of the request context.
3. DNS server returns address of a nearby replica.

Problem:

The DNS resolver may not be close to the actual client, so “nearest to resolver” may not mean “nearest to client.”

---

## CDN idea: Akamai-style caching

A CDN places caches/servers close to users.

Important note from the slides:

A cache may store more than passive files. Application code can also be moved closer to the user.

This improves:

- latency,
- bandwidth usage,
- scalability,
- availability.

---

## Code migration

Code migration means moving code from one machine to another.

Reasons:

| Reason | Explanation |
|---|---|
| Load distribution | Move work so servers are sufficiently loaded and energy is not wasted. |
| Move computation near data | Avoid sending huge data across network. |
| Flexibility | Move code to client when needed instead of pre-installing everything. |
| Privacy/security/legal limits | If data cannot move, move code to the data. |

Example: federated machine learning moves computation/model updates near data instead of collecting raw private data centrally.

---

## Code mobility components

An object or process can be seen as having:

| Component | Meaning |
|---|---|
| Code segment | Actual program instructions. |
| Data segment | State/data used by code. |
| Execution state | Current execution context of thread/process. |

---

## Weak vs strong mobility

| Type | What moves? | Meaning |
|---|---|---|
| Weak mobility | Code + data. | Execution restarts at destination. |
| Strong mobility | Code + data + execution state. | Execution continues from where it stopped. |

Weak mobility forms:

- **code shipping:** push code to another machine,
- **code fetching:** destination pulls code when needed.

Strong mobility forms:

- **migration:** move object/process completely,
- **cloning:** create a copy in same execution state.

---

## Migration in heterogeneous systems

Main problem:

Different machines may use different hardware, OS, runtime, and process/thread context representation.

Only practical solution:

Use an abstract machine implemented on different platforms.

Examples:

- interpreted languages with their own VM,
- virtual machine monitors.

Containers are much harder to migrate across heterogeneous OS environments because they depend on the underlying kernel.

---

## Migrating a virtual machine

Three alternatives:

1. **Pre-copy:** push memory pages to the new machine and resend pages modified during migration.
2. **Stop-and-copy:** stop the VM, copy memory, restart at destination.
3. **Post-copy / pull-on-demand:** start VM at destination and fetch pages as needed.

Tradeoff:

| Method | Advantage | Disadvantage |
|---|---|---|
| Pre-copy | Less downtime. | May resend dirty pages many times. |
| Stop-and-copy | Simple and consistent. | High downtime. |
| Pull-on-demand | Starts quickly at destination. | Page faults/network delay after start. |

Important warning:

A complete VM migration can take tens of seconds, and the service may be unavailable for multiple seconds.

---

## Exam check

1. What is request dispatching?
2. Why can the first tier become a bottleneck?
3. How does DNS support request dispatching for geographically distributed servers?
4. Why migrate code instead of data?
5. Compare weak mobility and strong mobility.
6. Compare three VM migration strategies.
