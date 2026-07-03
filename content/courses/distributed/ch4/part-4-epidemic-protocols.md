---
title: "Chapter 4, Part 4 – Epidemic Protocols and Deletion"
date: 2026-07-03
description: "Anti-entropy, rumor spreading, formal analysis, and death certificates."
tags: [distributed-systems, epidemic-protocols, gossip, chapter4]
toc: true
weight: 4
---

## Epidemic protocols

Epidemic protocols spread updates like infections.

Assumptions in slides:

- no write-write conflicts,
- updates occur at one server,
- replicas pass updated state to only a few neighbors,
- propagation is lazy, not immediate,
- eventually every replica should receive updates.

---

## Two forms of epidemics

| Form | Meaning |
|---|---|
| Anti-entropy | Replicas regularly choose another replica and exchange differences until both become consistent. |
| Rumor spreading | A newly updated replica tells other replicas; replicas may eventually stop spreading. |

---

## Anti-entropy operations

A node `P` selects another node `Q` at random.

| Mode | Meaning |
|---|---|
| Pull | `P` pulls missing updates from `Q`. |
| Push | `P` pushes its updates to `Q`. |
| Push-pull | Both exchange updates. |

Observation:

Push-pull takes about `O(log N)` rounds to disseminate updates to all `N` nodes.

---

## Anti-entropy analysis

Let `pi` be the probability that a node has not received the update after round `i`.

### Pull

```text
p(i+1) = p(i)^2
```

Reason: a node stays ignorant only if it was ignorant and contacts another ignorant node.

### Push

For small `pi` and large `N`:

```text
p(i+1) ≈ p(i)e^-1
```

Push is initially fast when many infected nodes spread the update, but becomes slower near the end because remaining ignorant nodes must be selected.

### Push-pull

Push-pull combines both advantages and spreads quickly.

---

## Rumor spreading

Model:

- server `S` has an update,
- it contacts other servers,
- if it contacts a server that already knows the update, `S` stops spreading with probability `pstop`.

Important formula from the slides:

```text
s = e^(-(1/pstop + 1)(1-s))
```

where `s` is the final fraction of ignorant servers.

---

## Effect of stopping

The slides show that with 10,000 nodes, larger `1/pstop` means fewer ignorant nodes.

Important conclusion:

> If every server must eventually receive the update, rumor spreading alone is not enough.

Reason: some nodes may never hear the rumor after spreaders stop.

---

## Deleting values

Deleting values is tricky with epidemic protocols.

Problem:

If a server simply removes an old value, another replica that still has the value may later spread it back.

Solution:

Use a **death certificate**.

A death certificate is a special update that says:

> This value was deleted; do not reintroduce it.

---

## When to remove death certificates

Death certificates cannot stay forever, but removing them too early is dangerous.

Options:

1. Run a global algorithm to detect that every server knows the deletion, then collect death certificates.
2. Give certificates a maximum lifetime, assuming they propagate within finite time.

Risk:

If a death certificate expires before reaching all servers, the deleted value may come back.

Exam sentence:

> In epidemic replication, deletion must be propagated as an update; otherwise old values can reappear.

---

## Exam check

1. Compare anti-entropy and rumor spreading.
2. Explain push, pull, and push-pull.
3. Why is push-pull `O(log N)`?
4. Why is rumor spreading alone not enough for guaranteed delivery?
5. What is a death certificate and why is it needed?
