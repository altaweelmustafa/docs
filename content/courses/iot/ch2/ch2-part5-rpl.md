---
title: "Chapter 2, Part 5 – RPL Protocol"
date: 2026-05-31
description: "Deep dive into RPL: DODAG formation, control messages, rank calculation, and storing vs non-storing modes."
tags: [iot, routing, rpl, dodag]
toc: true
---

## What Is RPL?

**RPL** (Routing Protocol for Low-Power and Lossy Networks) is a lightweight
distance-vector routing protocol designed specifically for IoT devices — constrained
sensors with limited memory, processing power, and battery.

A **lossy network** is one with a high packet drop rate, which is the norm in
wireless sensor environments. RPL is built to handle this gracefully.

RPL's core mechanism: it builds a **DODAG** (Destination-Oriented Directed Acyclic
Graph) — a tree-like, loop-free topology rooted at a single designated root node.

---

## The Root Node

The root is predefined by the network designer — typically a gateway, sink node, or
coordinator. It always has more power, better memory, and stable energy compared to
leaf nodes. The root of the DODAG always has **Rank = 0**.

---

## Control Messages: DIS, DIO, DAO, DAO-ACK

RPL uses four lightweight control messages to build and maintain the DODAG:

---

**DIS — DODAG Information Solicitation**

---

Sent by a new or disconnected node that wants to discover a nearby DODAG.

> *"Is there any DODAG near me?"*

Contains sender information, a request for DIO, and optional metrics.

---

**DIO — DODAG Information Object**

---

Sent by the root (and forwarded by other nodes) as an advertisement of the DODAG's
existence. The root multicasts DIO messages periodically.

> *"Yes, I am here."*

Contains: DODAG ID (root's IP address), Rank, Objective Function, Version Number,
configuration parameters, and routing metrics used for parent selection.

---

**DAO — Destination Advertisement Object**

---

Sent upward by a child node toward its selected parent (and eventually the root)
after joining the DODAG. This builds **downward routes** — so the root knows how to
reach each node later.

> *"I joined through you. Here is my info."*

| Field | Example | Meaning |
|---|---|---|
| Child Address | fe80::1234 | Address of the node sending the DAO |
| Preferred Parent | fe80::B | The parent selected to reach the root |
| Reachable Prefix | 2001:db8:1::/64 | Networks reachable through this node |
| Routing Information | Path via Parent B | Helps root send packets back downward |
| DAO Sequence Number | 25 | Identifies newer DAOs, discards stale ones |
| Flags | 0x02 | Control bits (e.g., request acknowledgment) |

---

**DAO-ACK — DAO Acknowledgement**

---

Sent by the parent or root in response to a DAO. Confirms successful registration
or rejects it.

> *"I received your DAO. (Success / Fail)"*

---

## Rank

**Rank** is a node's relative distance toward the root. Smaller rank = closer to
root = better position in the topology.

```
Rank(node) = Rank(parent) + Routing Cost
```

Rank must always **increase** as you move away from the root. This property prevents
loops — a defining feature of the DODAG structure.

Rank can depend on multiple factors depending on the Objective Function:

- Hop count
- ETX (Expected Transmission Count)
- Energy level
- Delay

---

## Routing Cost and Metrics

When a node receives DIO messages from multiple candidate parents, it must choose
the best one. The **Objective Function** transforms routing metrics into a comparable
cost value.

Common metrics:

| Metric | Goal | Description |
|---|---|---|
| ETX | Minimize | Expected number of transmissions for successful delivery |
| Delay / Latency | Minimize | Time for a packet to traverse the path |
| Link Quality | Maximize | Stability and reliability of the link (RSSI, PDR) |
| Hop Count | Minimize | Number of intermediate nodes to the root |
| Residual Energy | Maximize | Remaining battery on candidate parent |

**Example cost formula:**

```
Cost = W1*(Delay) + W2*(Hop Count) + W3*(ETX) + W4*(Link Quality)
```

With weights W1=0.4, W2=0.1, W3=0.3, W4=0.2:

- Parent A: Cost = 0.4(80) + 0.1(2) + 0.3(4) + 0.2(60) = **45.4**
- Parent B: Cost = 0.4(20) + 0.1(3) + 0.3(1) + 0.2(90) = **26.6**

Node X selects **Parent B** (lower cost).

Then: `Rank(Node X) = Rank(Parent B) + 26.6 = 10 + 26.6 = 36.6`

---

## Storing vs. Non-Storing Nodes

IoT nodes are severely constrained. Not all of them can maintain a full routing table.

**Storing nodes** keep a complete routing table — they know how to reach every other
node in the DODAG. This enables fast local forwarding but requires more memory,
processing, and energy. Gateways and routers are typically storing nodes.

**Non-storing nodes** only know their preferred parent. They do not keep a full
routing table. All routing decisions go through the root, which holds the complete
picture. This is ideal for small sensors and constrained devices — minimal memory,
minimal processing, low energy use.

> The root node is always a storing node.

---

## RPL Workflow: DODAG Formation Step by Step

1. Root broadcasts DIO messages advertising the DODAG.
2. A new node sends DIS: *"Is there any DODAG near me?"*
3. Neighboring nodes (potential parents) reply with DIO containing their metrics and rank.
4. The new node compares received DIOs using the Objective Function.
5. The new node selects the best parent.
6. The node sends a DAO upward to register its presence and build a downward route.
7. The parent/root replies with DAO-ACK confirming registration.
8. The newly joined node begins forwarding DIO messages to help expand the DODAG.
9. This repeats until all reachable nodes are connected and the DODAG is fully built.

**RPL keeps quiet when everything works correctly** — it uses a Trickle Timer
mechanism that reduces control message frequency once the network is stable, saving
energy during normal operation.

