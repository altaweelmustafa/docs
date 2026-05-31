---
title: "Chapter 2, Part 4 – Routing in IoT"
date: 2026-05-31
description: "How IoT routing protocols select paths, reduce energy, and extend network lifetime."
tags: [iot, routing, energy]
toc: true
---

## What Is Routing in IoT?

**Routing** is the process of selecting the path that data takes as it travels
between IoT devices, gateways, coordinators, controller services, or cloud servers.

Every routing decision directly affects four things:

- Communication delay.
- Energy consumption.
- Bandwidth utilization.
- Network reliability.

An IoT routing protocol must balance all four simultaneously, on devices with
severely constrained memory, processing, and battery.

---

## What a Routing Protocol Must Decide

- The best path for data transmission at any given moment.
- How to minimize transmission energy along that path.
- How to avoid congestion and packet loss.
- How to preserve network lifetime by not exhausting any single node's battery.

---

## Poor Routing vs. Good Routing

**Poor routing** (high-energy path):

- Longer distance between nodes → more transmission power required.
- More packet loss and retransmissions → additional energy burned.
- Higher delay and congestion.
- Nodes on the path drain their batteries faster, shortening network lifetime.

**Good routing** (energy-efficient path):

- Shorter or optimal path with lower transmission power.
- Fewer retransmissions.
- Lower delay and congestion.
- Balanced energy consumption across nodes → longer network lifetime and better QoS.

---

## Factors Considered in IoT Routing

| Factor | Description |
|---|---|
| Residual energy | Choose nodes with more remaining battery |
| Link quality | Prefer strong, stable links |
| Network load | Avoid routing through overloaded nodes |
| Hop count / distance | Balance hops and transmission energy |
| QoS requirements | Respect delay, reliability, and priority constraints |

---

## Common IoT Routing Solutions

**RPL (Routing Protocol for Low-Power and Lossy Networks)** — the standard
lightweight routing protocol for constrained IoT devices and wireless sensor
networks. Builds a tree-like topology toward a root node.

**Clustering-Based Routing (LEACH)** — uses cluster heads as aggregation points,
reducing the number of long-distance transmissions and distributing energy usage.

**Energy-Aware Routing** — explicitly selects paths based on remaining node battery
level and per-hop transmission cost, avoiding nodes near depletion.

**AI-Based Routing** — uses machine learning or optimization algorithms to
dynamically select efficient routes based on real-time network state.

---

*Previous: [Chapter 2, Part 3 – Load Balancing](/posts/ch2-part3-load-balancing)*  
*Next: [Chapter 2, Part 5 – RPL Protocol](/posts/ch2-part5-rpl)*
