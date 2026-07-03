---
title: "Chapter 5, Part 1 – Physical, Logical, and Vector Clocks"
date: 2026-07-03
description: "Clock synchronization, UTC, precision, accuracy, Lamport clocks, vector clocks, and causal delivery."
tags: [distributed-systems, clocks, lamport, vector-clocks, chapter5]
toc: true
weight: 1
---

## Physical clocks

Sometimes we need exact real time, not just event order.

The real-world reference is UTC: Universal Coordinated Time.

UTC is based on atomic clocks and is broadcast through radio/satellite systems.

---

## Precision vs accuracy

| Concept | Meaning | Formula |
|---|---|---|
| Precision | Clocks of different machines are close to each other. | `∀t, ∀p,q: |Cp(t)-Cq(t)| ≤ π` |
| Accuracy | A machine clock is close to real UTC time. | `∀t, ∀p: |Cp(t)-t| ≤ α` |

| Synchronization type | Goal |
|---|---|
| Internal synchronization | Keep clocks precise relative to each other. |
| External synchronization | Keep clocks accurate relative to UTC. |

---

## Clock drift

Hardware clocks are not perfect. They can run fast or slow.

A clock has maximum drift rate `ρ`.

If `F(t)` is hardware oscillator frequency and `F` is ideal frequency:

```text
1 - ρ ≤ F(t)/F ≤ 1 + ρ
```

The software clock follows hardware interrupts, so it inherits clock drift.

---

## Detecting and adjusting time using a time server

A client can ask a time server for time.

Times:

- `T1`: client sends request,
- `T2`: server receives request,
- `T3`: server sends response,
- `T4`: client receives response.

Assumption:

```text
T2 - T1 ≈ T4 - T3
```

Offset and delay can be estimated from these timestamps. Network Time Protocol (NTP) collects multiple `(offset, delay)` pairs and chooses the offset with minimal associated delay.

---

## Reference Broadcast Synchronization (RBS)

RBS idea:

1. A node broadcasts reference message `m`.
2. Each receiving node `p` records local receive time `Tp,m`.
3. Nodes compare receive times to estimate offsets.

Important: RBS focuses on receivers and removes sender uncertainty from the critical path.

The slides warn that simple averaging is not enough because drift changes over time, so linear regression is used:

```text
Offset[p,q](t) = αt + β
```

---

## Happened-before relation

Often exact time is less important than event order.

The happened-before relation `→` defines partial ordering:

1. If `a` and `b` are events in the same process and `a` occurs before `b`, then `a → b`.
2. If `a` is sending a message and `b` is receiving that message, then `a → b`.
3. If `a → b` and `b → c`, then `a → c`.

This gives only partial order because concurrent events may not be ordered.

---

## Lamport logical clocks

Goal: assign timestamp `C(e)` to each event `e` so that happened-before is respected.

Required properties:

- If `a → b` in same process, then `C(a) < C(b)`.
- If `a` sends message and `b` receives it, then `C(a) < C(b)`.

### Lamport rules

Each process `Pi` has local counter `Ci`.

1. Before each local event, increment `Ci`.
2. When `Pi` sends message `m`, attach timestamp `ts(m) = Ci`.
3. When `Pj` receives `m`, set `Cj = max(Cj, ts(m))`, then increment before delivering.
4. If timestamps tie, break ties with process ID.

### Important limitation

Lamport clocks guarantee:

```text
if a → b, then C(a) < C(b)
```

But they do **not** guarantee:

```text
if C(a) < C(b), then a → b
```

So Lamport clocks can order events, but cannot always prove causality.

---

## Vector clocks

Vector clocks solve more causality information than Lamport clocks.

Each process `Pi` maintains vector `VCi`.

Meaning:

- `VCi[i]` is `Pi`'s own logical clock,
- `VCi[j] = k` means `Pi` knows that `k` events occurred at `Pj`.

### Vector clock update rules

1. Before an event at `Pi`: `VCi[i] = VCi[i] + 1`.
2. When `Pi` sends message `m`, it attaches `ts(m) = VCi` after increment.
3. When `Pj` receives `m`, for each `k`:

```text
VCj[k] = max(VCj[k], ts(m)[k])
```

then `Pj` increments its own entry and delivers.

---

## Comparing vector timestamps

We say `a` may causally precede `b` if:

```text
for all k: ts(a)[k] ≤ ts(b)[k]
and for at least one k: ts(a)[k] < ts(b)[k]
```

Cases:

| Comparison | Meaning |
|---|---|
| `ts(a) < ts(b)` | `a` may causally precede `b`. |
| `ts(a) > ts(b)` | `b` may causally precede `a`. |
| neither `<` nor `>` | events are concurrent / may conflict. |

Example:

- `(2,1,0) < (4,3,0)` because every component is `≤` and at least one is `<`.
- `(4,1,0)` and `(2,3,0)` are incomparable because first component is larger in one vector, second component larger in the other.

---

## Causally ordered multicast

Goal:

> Deliver a message only after all causally preceding messages have been delivered.

Adjusted rule:

- increment vector clock only when sending,
- receiver adjusts vector clock when receiving.

A message `m` from `Pi` with timestamp `ts(m)` can be delivered at `Pj` only if:

```text
ts(m)[i] = VCj[i] + 1
and for all k ≠ i: ts(m)[k] ≤ VCj[k]
```

Interpretation:

1. The message is the next expected message from sender `Pi`.
2. All causal dependencies from other processes are already known at `Pj`.

---

## Example check

Given:

```text
VC3 = [0, 2, 2]
ts(m) = [1, 3, 0]
message from P1
```

Delivery test at `P3`:

1. Sender is `P1`, so check `ts(m)[1] = VC3[1] + 1`: `1 = 0 + 1`, true.
2. For other entries: `ts(m)[2] = 3 ≤ VC3[2] = 2` is false.

So `P3` must postpone delivery because it is missing causal information from `P2`.

---

## Exam check

1. Compare precision and accuracy.
2. What is clock drift?
3. Define happened-before relation.
4. Apply Lamport clock rules to a message send/receive.
5. Why do Lamport clocks not prove causality?
6. Compare two vector timestamps and decide if ordered or concurrent.
7. Apply causal multicast delivery conditions.
