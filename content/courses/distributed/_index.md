---
title: "Distributed Systems"
date: 2026-07-03
description: "Distributed systems COMP438 - Birzeit University"
tags: [distributed-systems, computer-science, birzeit, exam-notes]
toc: true
weight: 3
---

## What this course contains

These notes are written as a study website version of the uploaded Distributed Systems slides.  
They are organized into chapters and parts so you can study from the site instead of jumping through slides.

Covered uploaded chapters:

| Chapter | Topic | What you should master |
|---|---|---|
| Chapter 1 | Introduction | Design goals, transparency, dependability, security, scalability, system types, pitfalls |
| Chapter 3 | Processes | Threads, virtualization, clients, servers, server clusters, code migration |
| Chapter 4 | Communication | Layering, RPC, sockets, messaging, multicast, epidemic protocols |
| Chapter 5 | Coordination | Clock synchronization, logical/vector clocks, mutual exclusion, elections, gossip, event matching, location systems |

---

## How to study

1. Read the parts in order.
2. At the end of each part, answer the **Exam check** questions without looking.
3. Finish with the **Exam Cheatsheet** page.
4. For algorithms, memorize the **idea**, **steps**, **cost**, and **weakness**.

---

## Main rule of Distributed Systems

A distributed system tries to make many independent machines look like one coherent system, while hiding many ugly facts: latency, failures, different machines, different administrators, replication, synchronization, and partial knowledge.
