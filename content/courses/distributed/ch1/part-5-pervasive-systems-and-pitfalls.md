---
title: "Chapter 1, Part 5 – Pervasive Systems and Pitfalls"
date: 2026-07-03
description: "Ubiquitous, mobile, sensor, cloud-edge systems, and common false assumptions."
tags: [distributed-systems, pervasive, mobile, sensor-networks, pitfalls, chapter1]
toc: true
weight: 5
---

## Distributed pervasive systems

Pervasive systems are distributed systems where nodes are small, mobile, embedded, and naturally blended into the user environment.

They are often not obvious to the user. Instead of using one visible computer, users interact with many embedded or mobile devices.

Three overlapping subtypes:

1. Ubiquitous systems.
2. Mobile computing systems.
3. Sensor networks.

---

## Ubiquitous systems

A ubiquitous system surrounds the user and provides services without being intrusive.

Core elements:

| Element | Meaning |
|---|---|
| Distribution | Devices are networked, distributed, and transparently accessible. |
| Interaction | User interaction is unobtrusive. |
| Context awareness | System understands context and adapts. |
| Autonomy | Devices operate without constant human control. |
| Intelligence | System handles dynamic actions and interactions. |

Example: smart-home system that reacts to location, time, user habits, and device state.

---

## Mobile computing

Mobile computing involves devices whose location changes over time.

Distinctive features:

- many types of mobile devices,
- changing location,
- changing local services,
- changing reachability,
- discovery of nearby services,
- unstable communication.

Exam keyword: **discovery**. Mobile devices need to discover services/resources as they move.

---

## Mobile cloud and mobile edge computing

Mobile devices often cannot do all computation locally because of limited battery, CPU, storage, or network capacity.

| Concept | Meaning |
|---|---|
| Mobile cloud computing | Offload computation/storage to cloud data centers. |
| Mobile edge computing | Move computation closer to the user/device at the network edge. |

Why edge helps:

- lower latency,
- less backbone traffic,
- better support for location-aware services.

---

## Sensor networks

Sensor networks contain many simple nodes with sensors.

Characteristics:

- many nodes, from tens to thousands,
- limited memory and computation,
- limited communication,
- often battery-powered or battery-less,
- usually deployed to measure an environment.

---

## Sensor networks as distributed databases

A sensor network can be viewed as a distributed database:

- sensors produce data,
- queries ask for readings/events,
- system aggregates or forwards data.

Two extremes:

| Approach | Meaning |
|---|---|
| Store everything centrally | Send data to a sink/database. Easier queries but more communication. |
| Query inside the network | Process/aggregate near sensors. Saves communication but harder coordination. |

---

## Cloud-edge continuum

The cloud-edge continuum means computation/storage can happen at many levels:

```text
cloud data center  →  regional cloud  →  edge server  →  gateway  →  device/sensor
```

Tradeoff:

- Cloud: powerful, centralized, easier management, but higher latency.
- Edge: closer, faster response, context-aware, but more distributed and harder to manage.
- Device: immediate and private, but resource-constrained.

---

## Pitfalls in distributed-system design

Many distributed systems become complex because designers make false assumptions.

The famous false assumptions:

1. The network is reliable.
2. The network is secure.
3. The network is homogeneous.
4. The topology does not change.
5. Latency is zero.
6. Bandwidth is infinite.
7. Transport cost is zero.
8. There is one administrator.

### Why these assumptions are false

| Assumption | Reality |
|---|---|
| Network is reliable | Links fail, packets drop, machines crash. |
| Network is secure | Attackers may listen, modify, replay, or impersonate. |
| Network is homogeneous | Different OSs, hardware, protocols, data formats. |
| Topology does not change | Devices move, nodes join/leave, failures happen. |
| Latency is zero | Remote calls are slower than local calls. |
| Bandwidth is infinite | Large data transfer is costly and slow. |
| Transport cost is zero | Communication consumes money, energy, CPU, and time. |
| One administrator | Different organizations have different policies. |

---

## Exam check

1. What are the core elements of ubiquitous systems?
2. Why is mobile computing hard?
3. What is the difference between cloud and edge computing?
4. Why can sensor networks be seen as distributed databases?
5. List the eight false assumptions of distributed systems.
