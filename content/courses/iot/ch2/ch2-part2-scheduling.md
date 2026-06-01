---
title: "Chapter 2, Part 2 – Scheduling"
date: 2026-05-31
description: "How poor task scheduling wastes energy in IoT networks."
tags: [iot, energy, scheduling]
toc: true
---

## What Is Scheduling in IoT?

**Scheduling** is the mechanism that determines:

- *When* tasks execute.
- *Which node* executes them.

Done well, scheduling is one of the most powerful levers for reducing power
consumption. Done poorly, it becomes a major source of waste.

---

## What Poor Scheduling Looks Like

An IoT system with poor scheduling tends to exhibit these failure modes:

**Nodes never sleep.** If tasks are not coordinated, a node stays active all the
time — consuming full power when it could enter sleep mode between transmissions.
Sleep modes can reduce power consumption by orders of magnitude; poor scheduling
prevents nodes from ever reaching them.

**Idle listening.** A node that is awake but not receiving or transmitting anything
still draws significant power from its radio. Uncoordinated scheduling keeps radios
on unnecessarily.

**Collisions and retransmissions.** When multiple nodes transmit simultaneously
without a coordinated schedule, packets collide. Collisions force retransmissions,
and retransmissions consume additional energy — sometimes more than the original
transmission itself. Each collision is a small energy budget burned for nothing.

---

## Why It Compounds

The failure modes above reinforce each other. More retransmissions mean more active
radio time. More active radio time means less opportunity for sleep. Less sleep means
faster battery drain. Faster battery drain means shorter network lifetime — which
defeats the entire design goal of a long-lived IoT deployment.

A well-designed scheduling layer coordinates transmission windows, allows nodes to
enter sleep between windows, and distributes task assignment to balance load across
the network.

---

