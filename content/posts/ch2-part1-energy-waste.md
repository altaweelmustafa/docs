---
title: "Chapter 2, Part 1 – Energy Waste in IoT"
date: 2026-05-31
description: "Why energy efficiency matters in IoT and the primary causes of energy waste."
tags: [iot, energy, offloading]
toc: true
---

## Why Energy Efficiency Matters

IoT systems are always-on — sensors run continuously, usually on batteries, and
communication is the single largest consumer of that energy. At scale, inefficient
designs don't just shorten device lifetimes; they drive up operational cost and
ultimately collapse the network.

The core problems:

- Devices depend on batteries with no easy replacement path.
- Communication (radio transmission) dominates energy consumption.
- Large-scale deployments multiply every inefficiency.
- Without efficient protocols and routing, network lifetime collapses.

---

## Main Causes of Energy Waste

Energy inefficiency in IoT comes from multiple overlapping sources. Based on
literature, the main causes form around these axes:

**Offloading · Scheduling · Resource Management · Load Balancing · Node Deployment ·
Routing · Clustering · Changing Topology · Congestion · Limited Bandwidth · Latency**

This chapter walks through each one.

---

## Offloading

**Offloading** means moving selected tasks from sensors or edge devices to a fog,
edge, or cloud layer for processing.

The problem is not offloading itself — it is *bad* offloading decisions.

### When offloading saves energy

- The task is computationally heavy for the local device.
- Network transmission cost is low.
- The fog/cloud node is not already overloaded.
- Only filtered or pre-processed data is transmitted, not raw streams.

### When offloading wastes energy

- Raw data is sent unnecessarily when local filtering was possible.
- Network transmission cost is high relative to the computation saved.
- Tasks are forwarded to already-overloaded fog/cloud nodes.
- The offloading decision ignores latency, battery level, and available bandwidth.

Bad offloading decisions cascade: more raw data in transit means higher
communication overhead, longer processing times, increased delay, and more
transmission energy — exactly what offloading was supposed to reduce.

> **The problem is not offloading. The problem is bad offloading.**

---

*Next: [Chapter 2, Part 2 – Scheduling](/posts/ch2-part2-scheduling)*
