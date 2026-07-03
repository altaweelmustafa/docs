---
title: "Chapter 1, Part 1 – From Networked to Distributed Systems"
date: 2026-07-03
description: "Centralized, decentralized, distributed systems, common misconceptions, and perspectives."
tags: [distributed-systems, introduction, chapter1]
toc: true
weight: 1
---

## The big idea

A networked system is not automatically a distributed system. A distributed system is designed so that several independent computers cooperate and appear as one coherent system.

The chapter starts by comparing three shapes:

| Shape | Simple meaning | Risk |
|---|---|---|
| Centralized | One center controls or serves most things. | Bottleneck and single point of failure. |
| Decentralized | Multiple centers or clusters exist. | Still may not provide one coherent system. |
| Distributed | Nodes cooperate through a network as part of one system design. | Harder coordination, failure handling, and consistency. |

The slides ask: when does a decentralized system become distributed? The answer is not simply “when we add one more link.” A system becomes distributed because of its **behavior and design goals**, not only because of its graph shape.

---

## Two views on realizing distributed systems

| View | Meaning | Example |
|---|---|---|
| Integrative view | Connect existing networked computer systems into a larger system. | Integrating old business applications so they cooperate. |
| Expansive view | Extend an existing networked system with more computers. | Adding servers to scale an online service. |

The integrative view starts with already-existing systems and tries to make them work together. The expansive view starts with one system and grows it.

---

## Centralized does not always mean physically one machine

A common misconception is: “centralized solutions cannot scale.” The correct answer is more subtle.

A system can be:

- **Logically centralized:** there is one logical authority or namespace.
- **Physically distributed:** the implementation is spread across many machines.

Example: DNS root service is logically centralized because it represents one root of the naming system, but it is physically distributed across many servers.

Exam sentence:

> Centralization can be logical while implementation is physically distributed, so centralization does not always mean one machine.

---

## Perspectives on distributed systems

Distributed systems are complex, so we study them from several perspectives:

| Perspective | Main question |
|---|---|
| Architecture | How are components organized? |
| Process | What processes exist, and how do they relate? |
| Communication | How do components exchange data? |
| Coordination | How do components agree on order, access, time, or leadership? |

These are exactly the later chapters: processes, communication, and coordination.

---

## What to remember for the exam

A distributed system is not just “many computers connected.” It is a system where distribution is part of the design, and where the system must handle naming, communication, failures, consistency, scalability, and transparency.

---

## Exam check

1. What is the difference between centralized, decentralized, and distributed?
2. Why is DNS a good example of logical centralization but physical distribution?
3. What are the four perspectives used to study distributed systems?
