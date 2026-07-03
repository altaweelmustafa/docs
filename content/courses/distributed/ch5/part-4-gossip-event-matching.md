---
title: "Chapter 5, Part 4 – Gossip Coordination and Event Matching"
date: 2026-07-03
description: "Gossip aggregation, peer sampling, overlay construction, secure gossiping, publish-subscribe matching, Sub-2-Sub, and PEKS."
tags: [distributed-systems, gossip, publish-subscribe, event-matching, chapter5]
toc: true
weight: 4
---

## Gossip-based coordination

Gossip protocols let nodes coordinate through repeated random exchanges.

Typical applications:

- data dissemination,
- aggregation,
- peer sampling,
- overlay construction.

---

## Gossip aggregation

Each node `Pi` has value `vi`.

When two nodes gossip, both replace their values with the average:

```text
vi, vj ← (vi + vj) / 2
```

Eventually, all nodes converge to global average:

```text
v̄ = Σ vi / N
```

Example idea:

If one node starts with `1` and all others start with `0`, the final value at every node tends to `1/N`.

---

## Peer sampling

Problem:

Many gossip applications need to select a random peer from the whole network, but no node can know everyone in a huge system.

Solution:

- each node maintains a local list of `c` peer references,
- regularly chooses a peer from the list,
- exchanges about `c/2` references,
- when application needs a random peer, choose randomly from local list.

Observation from slides:

Statistically, choosing from the local list can become indistinguishable from choosing uniformly from the whole network.

---

## Gossip-based overlay construction

Goal: build a useful overlay using gossip.

Essence:

- maintain two local neighbor lists:
  - low-level list for peer sampling,
  - high-level list for application-specific neighbors.

### 2D torus example

Assume logical `N × N` grid.

Each node maintains list of `c` nearest neighbors.

Distance between `(a1,a2)` and `(b1,b2)`:

```text
d = d1 + d2
where di = min(N - |ai - bi|, |ai - bi|)
```

This wraps around like a torus.

Protocol idea:

1. Node picks random peer from low-level list.
2. It keeps closest nodes in high-level list.
3. Repeat rounds.
4. Overlay gradually becomes structured.

---

## Secure gossiping and hub attack

Attack:

Colluding nodes return only links to each other during reference exchanges.

Result:

Benign nodes may gradually receive more attacker links, causing attackers to become hubs.

In the slide scenario:

- 100,000 nodes,
- local list size `c = 30`,
- only 30 attackers,
- attackers can gain full control in less than 300 rounds.

---

## Defense: gathering statistics

Idea:

A benign node may use an exchange either to:

- update its local list, or
- gather statistics.

Attackers do not know which purpose is being used, so if statistical checking may reveal collusion, attackers are pressured to behave correctly.

One useful statistic:

- indegree distribution: how many nodes point to each node.

Unusually high indegree patterns may reveal hubs/colluders.

---

## Distributed event matching

Publish-subscribe idea:

- a process specifies subscriptions `S`,
- another process publishes notification `N`,
- system checks whether `S` matches `N`.

Hard part:

> Implement matching in a scalable way.

---

## Selective routing

Approach:

1. First broadcast subscriptions through the network.
2. Routers store filters in routing tables.
3. Notifications are forwarded only toward relevant subscribers/rendezvous nodes.

Example routing table idea:

| Interface | Filter |
|---|---|
| To node 3 | `a ∈ [0,3]` |
| To node 4 | `a ∈ [2,5]` |
| Toward router R1 | unspecified/general route |

---

## Sub-2-Sub gossiping

Goal:

Subscribers with similar interests should form one group, improving scalability.

Model:

- there are `N` attributes `a1, ..., aN`,
- attribute values map to floating-point numbers,
- subscription defines a region in N-dimensional space.

Example subscription:

```text
S = <a1 → 3.0, a4 → [0.0, 0.5)>
```

Meaning:

- `a1` must equal `3.0`,
- `a4` must be between `0.0` and `0.5`,
- other attributes do not matter.

A notification matches if it falls in the subscription region.

---

## Secure publish-subscribe dilemma

Publish-subscribe wants decoupling:

- publishers and subscribers may not know each other,
- anonymity may be needed,
- but security normally uses secure channels between known parties.

Problems:

- referential decoupling prevents simple secure channels,
- unknown source creates integrity problems,
- trusted broker may be impossible for sensitive data.

---

## PEKS: Public-Key Encryption with Keyword Search

Goal:

Search/match encrypted data without decrypting full messages at the server.

A message `m` with keywords `KW1 ... KWn` is stored as:

```text
m* = [PK(m) | PEKS(PK, KW1) | PEKS(PK, KW2) | ... | PEKS(PK, KWn)]
```

A subscriber gets a secret key and can generate a trapdoor for a keyword.

The server can test whether encrypted keywords match the trapdoor without learning the plaintext keyword/message.

Exam summary:

> PEKS lets a system route/search encrypted messages using encrypted keywords.

---

## Exam check

1. How does gossip aggregation compute an average?
2. What is peer sampling and why is it useful?
3. How does the 2D torus overlay use distance?
4. What is a hub attack in gossip?
5. What is distributed event matching?
6. Why is secure publish-subscribe difficult?
7. What does PEKS solve?
