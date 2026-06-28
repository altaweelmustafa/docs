---
title: "Chapter 7 – Deadlocks"
date: 2026-06-28
weight: 9
toc: true
tags:
  [
    "operating-systems",
    "deadlock",
    "resource-allocation",
    "banker's-algorithm",
    "deadlock-prevention",
    "deadlock-avoidance",
  ]
description: "Deadlock definition, system model, the four necessary conditions, resource allocation graphs, deadlock prevention, and deadlock avoidance with the Banker's Algorithm."
author: "Mustafa Altaweel"
---

## Deadlock — Definition

> **Deadlock**: a set of waiting (blocked) processes where each process is holding a resource and waiting for other processes to release their resources.

**Simple example:**

```
        PR-1
      ↗       ↖
   P₁   (cycle)  P₂
      ↘       ↗
        PR-2
```

P₁ holds PR-1 and waits for PR-2. P₂ holds PR-2 and waits for PR-1 → neither can proceed.

---

## System Model

- We have resource types: R₀, R₁, ..., Rₙ₋₁ (e.g. HD, tapes, printers).
- We have Wⱼ instances of each resource type Rⱼ.
- Each process uses resources in the following order:
  1. **Request** the resource.
  2. **Use** the resource.
  3. **Release** the resource.

### Deadlock Handling

The OS handles deadlocks in one of two ways:

1. **Allow** the system to enter a deadlock and then **recover** from it (used in UNIX).
2. **Prevent** the system from entering a deadlock state.

---

## Necessary Conditions for Deadlock

All **four** of the following conditions must hold **simultaneously** for a deadlock to occur:

| Condition                | Description                                                                                                                                                                       |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **(1) Mutual Exclusion** | The resource type must be used exclusively — it cannot be shared by more than one process at a time.                                                                              |
| **(2) Hold & Wait**      | Each process is holding a resource type and waiting for another process to release the resource of the same type.                                                                 |
| **(3) No Pre-emption**   | Cannot remove (forcibly take away) any of the resources from a process holding them.                                                                                              |
| **(4) Circular Wait**    | There exists a sequence of processes ⟨P₀, P₁, P₂, ..., Pₙ₋₁⟩ such that P₀ waits for P₁, P₁ waits for P₂, ..., Pₙ₋₂ waits for Pₙ₋₁, and Pₙ₋₁ waits for P₀. This forms a **cycle**. |

---

## Resource Allocation Graph

A graph G = (V, E) where:

- **V** = set of vertices = **{P: processes}** ∪ **{R: resource types}**
- **E** = set of edges:
  - **(Pᵢ, Rⱼ)**: process Pᵢ is **requesting** one instance of resource type Rⱼ.
  - **(Rⱼ, Pᵢ)**: one instance of resource type Rⱼ is **allocated** to process Pᵢ.

### Example

```
P = {P₁, P₂, P₃}
R = {R₁(1), R₂(2), R₃(1), R₄(3)}
E = {(P₁,R₁), (P₂,R₃), (R₁,P₂), (R₂,P₂), (R₂,P₁), (R₃,P₃)}
```

> If the graph **contains a cycle** → possible deadlock.
> If no cycle → no deadlock.
> If each resource type has exactly **one instance** → cycle means **definite deadlock**.

---

## Deadlock Prevention

To prevent deadlock, ensure at least **one** of the four necessary conditions cannot hold:

### 1 — Mutual Exclusion

By default, some resources are mutually exclusive (e.g. printers) and we can't do anything about it. This condition generally **cannot be prevented**.

### 2 — Hold & Wait

To break this condition:

- **(I)** Let the process request **all its resources at the beginning** before it starts.
- **(II)** The process is granted all its resources only when it has **none** currently.

> **Problem**: Starvation — a process needing many popular resources may wait forever.

### 3 — No Pre-emption

If a process requests a resource that is not available, it **must release** all the resources it currently holds.

> **Problem**: Low system utilization (poor performance), in addition to starvation.

### 4 — Circular Wait

Assign a global ordering to all resource types: ①Card Reader ②Hard Disk ③Tape ④Printer.

Each process can only request resources in **increasing order** of enumeration.

**Implementation for Dining Philosophers (breaking circular wait):**

```c
semaphore s[i] = {1, 1, 1, 1, 1};

// Philosopher i:
Repeat
    Think;
    wait(S[min(i, (i+1)%5)]);
    wait(S[max(i, (i+1)%5)]);
    Eat;
    signal(S[(i+1)%5]);
    signal(S[i]);
Until False;
```

---

## Deadlock Avoidance

Rather than preventing deadlock structurally, the OS **dynamically** decides whether to grant a resource request based on whether the system will remain in a **safe state**.

### Safe State

> **Definition**: A system is in a **safe state** if there exists a sequence of processes ⟨P₀, P₁, P₂, ..., Pₙ₋₁⟩ such that:
>
> - P₀ can take all available resources, execute, and finish.
> - P₁ can take all available resources + resources released by P₀, execute, and finish.
> - P₂ can take all available + resources released by P₀, P₁, execute, and finish.
> - ...and so on until Pₙ₋₁.
>
> **If such a sequence exists → system is SAFE → No deadlock.**

### Example (single resource type: tape units)

12 tape units, 3 processes. Snapshot:

| Process | Max Needs | Allocated | Current Needs |
| ------- | --------- | --------- | ------------- |
| P₀      | 10        | 5         | 5             |
| P₁      | 4         | 2         | 2             |
| P₂      | 9         | 3         | 7             |

Available = 12 − (5+2+3) = **3**

Safe sequence: ⟨P₁, P₀, P₂⟩

- Available: 3 → P₁ (needs 2) → finishes, releases 2 → available = 5
- Available: 5 → P₀ (needs 5) → finishes, releases 5 → available = 10
- Available: 10 → P₂ (needs 7) → finishes → **SAFE** ✓

If P₂ requests 1 more tape → available = **2**:

- No safe sequence exists → **deadlock** ✗

---

## Banker's Algorithm

For **deadlock avoidance** with **multiple resource types**. Each process declares its maximum resource needs at the start.

### Data Structures

- **Max[i][j]** = maximum number of instances of resource type j that process Pᵢ may need.
- **Allocation[i][j]** = number of instances of resource type j currently allocated to Pᵢ.
- **Need[i][j]** = remaining resource need of Pᵢ.
  - `Need[i][j] = Max[i][j] − Allocation[i][j]`
- **Available[j]** (= W) = number of available instances of resource type j.

### Algorithm Steps

```
1. Let W = Available
2. Define array K[i] = 1 for all i = 0, 1, ..., n−1
3. Find an i such that:
       K[i] == 1  AND  Need[i] ≤ W
   If no such i exists → Go to Step 5
4. W = W + Allocation[i]
   K[i] = 0
   Go to Step 3
5. If K[i] == 0 for all i → System is SAFE
   else → System is UNSAFE
```

### Example

| Process | Max | Allocation | Need |
| ------- | --- | ---------- | ---- |
| P₀      | 10  | 5          | 5    |
| P₁      | 4   | 2          | 2    |
| P₂      | 9   | 2          | 7    |

Available (W) = 3

**Step-by-step:**

| Step             | W       | K array |
| ---------------- | ------- | ------- |
| Start            | 3       | [1,1,1] |
| P₁ (Need=2 ≤ 3)  | 3+2=5   | [1,0,1] |
| P₀ (Need=5 ≤ 5)  | 5+5=10  | [0,0,1] |
| P₂ (Need=7 ≤ 10) | 10+2=12 | [0,0,0] |

All K[i] = 0 → **System is SAFE**. Safe sequence: ⟨P₁, P₀, P₂⟩.

### Multi-Resource Banker's Example

| Process | Allocation (A B C) | Max (A B C) | Need (A B C) |
| ------- | ------------------ | ----------- | ------------ |
| P₀      | 0 1 0              | 7 5 3       | 7 4 3        |
| P₁      | 2 0 0              | 3 2 2       | 1 2 2        |
| P₂      | 3 0 2              | 9 0 2       | 6 0 0        |
| P₃      | 2 1 1              | 2 2 2       | 0 1 1        |
| P₄      | 0 0 2              | 4 3 3       | 4 3 1        |

Total = (10, 5, 7). Available = (3, 3, 2).

Safe sequence: **⟨P₁, P₃, P₀, P₂, P₄⟩**

If P₄ requests (3,3,0): Available becomes (0,0,2) → **Not Safe**.
