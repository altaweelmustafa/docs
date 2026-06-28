---
title: "Chapter 5 – CPU Scheduling"
date: 2026-06-28
weight: 7
toc: true
tags:
  [
    "operating-systems",
    "cpu-scheduling",
    "FCFS",
    "SJF",
    "priority",
    "round-robin",
    "multilevel-queues",
  ]
description: "CPU scheduling criteria, preemptive vs non-preemptive scheduling, and all six algorithms: FCFS, SJF, Priority, Round Robin, Multi-Level Queues, and Multi-Level Feedback Queues."
author: "Mustafa Altaweel"
---

## CPU Scheduling

**CPU Scheduling** is the process or decision by which the OS selects a process from the **Ready Queue** and gives it to the CPU to execute. This is **Short-Term Scheduling**.

### Cases That Invoke CPU Scheduling

1. **I/O required** — process leaves CPU to wait for I/O.
2. **Interrupt** — another process preempts the running one.
3. **Process I/O is completed** — (In Synchronous Mode) returns to Ready Queue.
4. **Process terminated** — CPU becomes free.

> Cases **1 & 4** are **Non-Preemptive** — the CPU is taken away only when the process voluntarily gives it up.
> Cases **2 & 3** are **Preemptive** — the CPU can be forcibly taken away and given to another process.

---

## Scheduling Criteria

Our objective is to evaluate all scheduling algorithms against these criteria:

| Criterion           | Goal | Description                                                                   |
| ------------------- | ---- | ----------------------------------------------------------------------------- |
| **CPU Utilization** | MAX  | Keep the CPU as busy as possible                                              |
| **Throughput**      | MAX  | Number of jobs finishing per unit time                                        |
| **Turnaround Time** | MIN  | Time from submitting a job until it finishes execution                        |
| **Waiting Time**    | MIN  | Time the process spends in the Ready Queue                                    |
| **Response Time**   | MIN  | Time from submitting a job until you see the first response from the computer |

**Key formulas:**

```
Turnaround Time  = Finish Time − Arrival Time
Waiting Time     = Turnaround Time − Service (CPU) Time
Weighted Turnaround Time = Turnaround Time / Service (CPU) Time   (minimum is better)
```

> Every switching from process Pᵢ to Pⱼ needs **2 Context Switches**.

---

## [1] FCFS — First Come First Serve

Processes are served in the order they arrive. **Non-preemptive**.

### Example

| Process | Arrival Time | Service Time (CPU burst) |
| ------- | ------------ | ------------------------ |
| P₁      | 0            | 3                        |
| P₂      | 2            | 5                        |
| P₃      | 4            | 1                        |
| P₄      | 5            | 4                        |
| P₅      | 8            | 1                        |

**Gantt Chart:**

```
| P₁ | P₂       | P₃ | P₄    | P₅ |
0    3    4    5    8    9         13   14
```

- **Average Turnaround Time** = [(3−0)+(8−2)+(9−4)+(13−5)+(14−8)] / 5 = **5.6 units**
- **Average Waiting Time** = [(3−0−3)+(8−2−5)+(9−4−1)+(13−5−4)+(14−8−1)] / 5 = **2.8 units**

### FCFS: Convoy Problem

A long job at the front makes all shorter jobs wait. Example:

| Process | CPU burst |
| ------- | --------- |
| P₁      | 1         |
| P₂      | 5         |
| P₃      | 27        |

If order is P₁, P₂, P₃ (shortest first):

- ATT = (1−0)+(6−0)+(33−0) / 3 = **40/3**
- AWT = (1−0−1)+(6−0−5)+(33−0−27) / 3 = **7/3**

If order is P₃, P₂, P₁ (longest first):

- ATT = (27−0)+(32−0)+(33−0) / 3 = **92/3**
- AWT = (27−0−27)+(32−0−5)+(33−0−1) / 3 = **59/3**

> Clearly FCFS suffers when a large job blocks small ones.

---

## [2] Shortest Job First (SJF)

The CPU is given to the process with the **smallest CPU burst (service time)**.

> **Note: SJF gives the minimum (optimal) average waiting time.**

### Example (all arrive at time 0)

| Process | CPU burst |
| ------- | --------- |
| P₁      | 24        |
| P₂      | 3         |
| P₃      | 3         |

**Gantt Chart (order: P₂, P₃, P₁):**

```
| P₂ | P₃ | P₁          |
0    3    6             30
```

- ATT = (3+6+30)/3 = **39/3 = 13**
- AWT = (0+3+6)/3 = **9/3 = 3**

### Two Versions of SJF

| Version                                                   | Behavior                                                                                                                                            |
| --------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| **(1) Preemptive — SRTF** (Shortest Remaining Time First) | If a new job arrives with a CPU burst less than the **remaining** time of the running process, the CPU **switches** to the new process immediately. |
| **(2) Non-Preemptive**                                    | If a new job arrives with a shorter burst, the CPU **finishes** the current process first, then switches.                                           |

### Example (Preemptive vs Non-Preemptive)

| Process | Arrival Time | CPU Time |
| ------- | ------------ | -------- |
| P₁      | 0            | 7        |
| P₂      | 2            | 4        |
| P₃      | 4            | 1        |
| P₄      | 5            | 4        |

**(a) Preemptive (SRTF):**

```
| P₁ | P₂ | P₃ | P₂ | P₄    | P₁          |
0    2    4    5    7         11            16
```

- ATT = [(16−0)+(7−2)+(5−4)+(11−5)] / 4 = **12/4 = 3**
- AWT = [(16−0−7)+(7−2−4)+(5−4−1)+(11−5−4)] / 4 = **3**

**(b) Non-Preemptive:**

```
| P₁     | P₃ | P₂    | P₄      |
0    2    4  5   7    8         12            16
```

- ATT = [(7−0)+(12−2)+(8−4)+(16−5)] / 4 = **8**
- AWT = [(7−0−7)+(12−2−4)+(8−4−1)+(16−5−4)] / 4 = **18/4**

### SJF Problems

- **Starvation**: long jobs may never run if short jobs keep arriving.
- **Solution**: **Aging** — as time progresses, give the process some additional priority.
- **Major Problem**: How can the OS decide the length of the next CPU burst?
- **Solution**: The OS can only **estimate** the length of the next CPU burst using the formula:

```
Yₙ₊₁ = w × Tₙ + (1 − w) × Yₙ
```

Where:

- `Tₙ` = actual length of the n-th CPU burst
- `Yₙ` = estimated length of the n-th CPU burst
- `w` = constant, 0 ≤ w ≤ 1
  - If w = 0 → Yₙ₊₁ = Yₙ (ignore actual, keep old estimate)
  - If w = 1 → Yₙ₊₁ = Tₙ (use only actual, ignore history)

**Expanded formula (substituting w = 1/2):**

```
Yₙ₊₁ = Tₙ/2 + Tₙ₋₁/2² + Tₙ₋₂/2³ + Tₙ₋₃/2⁴ + ...
```

---

## [3] Priority Scheduling

The CPU is given to the process with the **highest priority**. Every process is assigned a priority number. Generally, **low number = low priority**.

> **System tasks (interrupts) have the highest priority.**

Two versions: **(a) Preemptive** and **(b) Non-Preemptive**.

### Example (high number = high priority)

| Process | CPU burst | Priority | Arrival |
| ------- | --------- | -------- | ------- |
| P₁      | 10        | 1        | 10:00   |
| P₂      | 52        | 2        | 10:02   |
| P₃      | 10        | 5        | 10:05   |
| P₄      | 8         | 4        | 10:08   |

**(a) Preemptive:** P₃ preempts whoever is running when it arrives at 10:05 (highest priority).

**(b) Non-Preemptive:** Finishes current job then picks highest priority next.

### Problem & Solution

- **Problem**: **Starvation** — low priority processes may never run.
- **Solution**: **Aging** — as time progresses, increase the priority of waiting processes.

---

## [4] Round Robin (RR)

Best designed for **time sharing / interactive systems**.

- Each process is assigned a time slice called **quantum Q**.
- The process runs for this quantum; CPU then switches to the next process on a **FCFS basis**.
- CPU also switches early if the job needs I/O or finishes.

**If Q = very big** → degenerates to **FCFS**.
**If Q = very small** → too many context switches, overhead increases.

### Example (Q = 20)

| Process | Service Time |
| ------- | ------------ |
| P₁      | 53           |
| P₂      | 17           |
| P₃      | 68           |
| P₄      | 24           |

**Gantt Chart:**

```
| P₁ | P₂ | P₃ | P₄ | P₁ | P₃  | P₄ | P₁ | P₂ | P₃  |
0   20   37   57   77   97  117  121  141  161 164  172
```

P₂ finishes at 37, P₄ finishes at 121, P₁ finishes at 161, P₃ finishes at 172.

---

## [5] Multi-Level Queues

The Ready Queue is divided into **several queues**. Each queue has its own scheduling algorithm.

**Scheduling between queues** (how to distribute CPU time among queues) uses two approaches:

1. **Time Slice**: each queue is assigned a chunk/slice of CPU time, shared among its processes.
2. **Fixed Priority**: serve all jobs in the highest queue first, then the next, etc. (can be preemptive or non-preemptive).

### Classic 3-Queue Example

| Time Slice | Algorithm | Queue                           |
| ---------- | --------- | ------------------------------- |
| 400 ms     | RR, Q=100 | System Tasks (highest priority) |
| 100 ms     | RR, Q=10  | Interactive Jobs                |
| 20 ms      | FCFS      | Batch Jobs (lowest priority)    |

> **Problem**: Starvation of lower-priority queues.

---

## [6] Multi-Level Feedback Queues

Same as Multi-Level Queues, but **processes can move up and down between queues**.

- New process always enters the **highest-priority queue (Q1)**.
- If it uses its full quantum → moves **down** to Q2.
- If it uses Q2's quantum fully → moves down to Q3 (FCFS).
- If it yields CPU early (I/O) → stays or moves **up**.

### Example Structure

```
New process → Q1 (RR, Q=10ms)  → if not done → Q2
              Q2 (RR, Q=100ms) → if not done → Q3
              Q3 (FCFS)
```

### Example (Q1=10, Q2=100, Q3=FCFS)

| Process | CPU burst |
| ------- | --------- |
| P₁      | 25        |
| P₂      | 160       |
| P₃      | 120       |
| P₄      | 8         |

**Gantt Chart:**

```
| P₁ | P₂ | P₃ | P₄ | P₁ | P₂  | P₃  | P₂  |    P₃   |
0   10   20   30   38   53  153  253  303  313
```

P₄ finishes at 38, P₁ finishes at 53, P₂ finishes at 303, P₃ finishes at 313.

---

## Algorithm Evaluation

| Method                  | Quality     | Description                                            |
| ----------------------- | ----------- | ------------------------------------------------------ |
| (1) Deterministic Model | Poor        | Uses a specific predetermined workload                 |
| (2) Queuing Theory      | Theoretical | Mathematical analysis                                  |
| (3) Simulation          | Good        | Models real workloads                                  |
| **(4) Implementation**  | **Best**    | Actually implement and test the algorithm in a real OS |
