---
title: "Chapter 4, Part 3 – Multicast Communication"
date: 2026-07-03
description: "Application-level multicast, Chord-based multicast, link stress, stretch, and flooding."
tags: [distributed-systems, multicast, flooding, chapter4]
toc: true
weight: 3
---

## Application-level multicasting

Multicasting means sending data to a group.

In application-level multicast, nodes organize themselves into an overlay network and use that overlay to disseminate data.

Common overlay structures:

| Structure | Meaning |
|---|---|
| Tree | Unique paths from root to members. Simple and efficient but vulnerable to tree failures. |
| Mesh | Multiple paths, more robust, but needs routing and duplicate control. |

---

## Chord-based application-level multicast

Basic approach from slides:

1. Initiator generates multicast identifier `mid`.
2. Lookup `succ(mid)`, the node responsible for `mid`.
3. Request routes to `succ(mid)`, which becomes tree root.
4. If process `P` wants to join, it sends join request to root.
5. When request arrives at `Q`:
   - if `Q` has not seen a join request before, it becomes a forwarder and `P` becomes child of `Q`,
   - if `Q` already knows about the tree, `P` becomes child of `Q` and request stops.

Main idea:

> The overlay routing path is reused to build the multicast tree.

---

## Cost metrics for ALM

| Metric | Meaning |
|---|---|
| Link stress | How many times the same physical link carries the same multicast message. |
| Stretch | Ratio between overlay path delay and direct network-level path delay. |

Formula:

```text
stretch = ALM path length / network-level path length
```

Example from slides:

If overlay path length is `73` and network path length is `47`:

```text
stretch = 73 / 47
```

Low stress and low stretch are better.

---

## Flooding

Flooding is a simple multicast method.

Rule:

1. Node `P` sends message `m` to all neighbors.
2. Each neighbor forwards `m` to its neighbors except the sender.
3. A node forwards only if it has not seen `m` before.

Advantages:

- simple,
- robust,
- no tree construction needed.

Disadvantages:

- can create many duplicate messages,
- expensive in dense networks,
- needs message IDs/history to avoid loops.

---

## Probabilistic flooding

Variation:

A node forwards a message with probability `pflood`.

This probability may depend on:

- node degree,
- neighbor degree,
- desired reliability/overhead tradeoff.

Tradeoff:

- higher `pflood` → better reachability but more overhead,
- lower `pflood` → less overhead but higher chance some nodes miss message.

---

## Exam check

1. What is application-level multicast?
2. Why are trees common in multicast overlays?
3. Explain link stress and stretch.
4. How does flooding avoid infinite loops?
5. What is the tradeoff in probabilistic flooding?
