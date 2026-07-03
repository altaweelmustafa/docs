---
title: "Chapter 5, Part 3 – Election Algorithms"
date: 2026-07-03
description: "Coordinator election, bully algorithm, ring algorithm, ZooKeeper, Raft, proof of stake, and wireless elections."
tags: [distributed-systems, election, raft, zookeeper, chapter5]
toc: true
weight: 3
---

## Why election algorithms?

Some distributed algorithms need one process to act as coordinator.

Problem:

> If the coordinator fails, how do we dynamically choose a new one?

Manual selection creates a single point of failure. Election algorithms let processes agree on a new coordinator.

---

## Basic assumptions

Traditional election algorithms often assume:

1. All processes have unique IDs.
2. All processes know all process IDs.
3. A process may not know which processes are currently up.
4. Election means selecting the highest-ID process that is alive.

---

## Bully algorithm

Assume processes are `P0, P1, ..., PN-1` and ID of `Pk` is `k`.

When `Pk` notices the coordinator is not responding:

1. `Pk` sends `ELECTION` messages to all higher-ID processes.
2. If nobody responds, `Pk` wins and becomes coordinator.
3. If a higher-ID process responds, that higher process takes over the election.

Why “bully”?

The highest-ID alive process always wins, pushing lower processes out.

Advantages:

- simple,
- elects highest-ID alive process.

Disadvantages:

- assumes known membership,
- many messages in worst case,
- not ideal for very large or dynamic systems.

---

## Ring election algorithm

Processes are organized in a logical ring.

Principle:

- priority is based on process ID/priority,
- highest priority alive process becomes coordinator.

Steps:

1. Any process starts election by sending election message to successor.
2. If successor is down, pass message to next successor.
3. Each process that forwards message adds itself to the list.
4. When message returns to initiator, all alive processes are known.
5. Initiator sends coordinator message around ring with list/living winner.
6. Highest-priority process becomes coordinator.

Advantages:

- does not need to contact all processes directly at once,
- works naturally with logical ring.

Disadvantages:

- ring maintenance is needed,
- failures complicate successor selection,
- can take time to circulate.

---

## ZooKeeper leader election

Each server `s` has:

- unique identifier `id(s)`,
- transaction counter `tx(s)`: latest transaction handled.

When follower suspects leader crashed, it broadcasts:

```text
(voteID, voteTX)
```

Initially:

```text
voteID = id(s)
voteTX = tx(s)
```

Each server keeps:

| Variable | Meaning |
|---|---|
| `leader(s)` | server believed to be final leader. |
| `lastTX(s)` | most recent transaction known to `s`. |

### Receiving a vote

When `s*` receives `(voteID, voteTX)`:

1. If `lastTX(s*) < voteTX`, the received vote has more up-to-date transaction info:
   - set `leader(s*) = voteID`,
   - set `lastTX(s*) = voteTX`.
2. If `lastTX(s*) = voteTX` and `leader(s*) < voteID`, transaction freshness ties, so higher ID wins:
   - set `leader(s*) = voteID`.

If a server believes it should lead, it broadcasts its own `(id, tx)`.

Exam summary:

> ZooKeeper election prefers the server with the most recent transaction; if tied, it prefers the higher ID.

---

## Raft leader election

Raft uses a small group of servers. Each server is in one of three states:

| State | Meaning |
|---|---|
| Follower | Passive; listens to leader/candidates. |
| Candidate | Wants to become leader. |
| Leader | Sends heartbeats and coordinates. |

Raft works in **terms**, starting at term `0`.

All servers start as followers.

Leader regularly sends heartbeat messages.

---

## Selecting a new Raft leader

If a follower does not hear from the leader for some time:

1. It increments term.
2. It becomes candidate.
3. It broadcasts a vote request.

Responses:

- If old leader is still alive and receives message, it says it is still leader; candidate returns to follower.
- If another follower receives the first election message in current term, it votes for candidate.
- Otherwise it ignores duplicate election messages.
- Candidate wins when it receives majority votes.

Important observation:

Randomized/different timeouts reduce concurrent elections and help convergence.

---

## Proof-of-stake election

Assume a blockchain system with `N` secure tokens.

Properties:

- each token has unique owner,
- each token has unique index `1 ≤ k ≤ N`,
- token cannot be copied/modified unnoticed.

Leader selection:

1. draw random number `k` from `{1, ..., N}`,
2. find process owning token `k`,
3. that process becomes next leader.

Observation:

> More tokens means higher probability of being selected.

---

## Wireless network leader election

In wireless environments, a leader may be selected based on capacity.

Idea:

- find the node with highest capacity,
- nodes report back only the highest-capacity node they found.

This reduces communication and picks a strong coordinator for the environment.

---

## Exam check

1. Why do we need election algorithms?
2. Explain bully algorithm step by step.
3. Explain ring election step by step.
4. In ZooKeeper, what wins first: higher transaction or higher ID?
5. What are Raft follower/candidate/leader states?
6. Why do different timeouts help Raft elections?
7. How does proof-of-stake select a leader?
