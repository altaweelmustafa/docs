---
title: "Chapter 6 – Concurrent Processes & Process Synchronization"
date: 2026-06-28
weight: 8
toc: true
tags:
  [
    "operating-systems",
    "concurrency",
    "synchronization",
    "critical-section",
    "semaphores",
    "deadlock",
    "mutex",
  ]
description: "Precedence graphs, Bernstein's conditions, Fork/Join, Parbegin/Parend, the Critical Section Problem, software and hardware solutions, semaphores, and the classical synchronization problems."
author: "Mustafa Altaweel"
---

## Concurrent Processes

Concurrent processes are either:

- **Independent**: cannot affect or be affected by the execution of another process.
- **Cooperating**: can affect or be affected by another process (share data).

---

## Precedence Graph

A **precedence graph** is a directed graph whose nodes correspond to statements. An edge from node Sᵢ to node Sⱼ means that Sⱼ is only executed **after** Sᵢ completes.

**Example:** Given statements:

```
(1) a = x + y
(2) b = z + 1
(3) c = a - b
(4) w = c + 1
```

- (1) and (2) **can** be executed concurrently (no dependency).
- (3) depends on both (1) and (2) → cannot run concurrently with either.
- (4) depends on (3) → cannot run concurrently with (3).

---

## Concurrency Condition — Bernstein's Conditions

**Define:**

- `R(Sᵢ)` = READ set — all variables **read** by statement Sᵢ.
- `W(Sᵢ)` = WRITE set — all variables **written** by statement Sᵢ.

**Two statements S₁ and S₂ can execute concurrently if and only if:**

```
R(S₁) ∩ W(S₂) = ∅
W(S₁) ∩ R(S₂) = ∅
W(S₁) ∩ W(S₂) = ∅
```

**Example:**

```
S₁: a = x + y     R(S₁) = {x,y},  W(S₁) = {a}
S₂: b = z + 1     R(S₂) = {z},    W(S₂) = {b}
```

- `{x,y} ∩ {b} = ∅` ✓
- `{a} ∩ {z} = ∅` ✓
- `{a} ∩ {b} = ∅` ✓ → **S₁ and S₂ can run concurrently.**

**Counter-example:**

```
S₃: c = a - b     R(S₃) = {a,b}
R(S₃) ∩ W(S₂) = {a,b} ∩ {b} ≠ ∅ → Cannot run concurrently.
```

---

## Fork & Join Constructs

Since precedence graphs are difficult to use directly in programs, **Fork** and **Join** are used.

- **Fork L**: splits one computation into two concurrent computations. One starts at label L, the other continues after the fork.
- **Join**: recombines two concurrent computations. The first to finish waits for the other.

```
count = number of computations to join;
function join:
{
    count = count - 1;
    if (count != 0) quit (stop this computation);
}
```

> **Very Important Note**: The `join` statement must be executed **atomically** — one process at a time; it cannot be executed concurrently.

### Example (two statements from before, using Fork/Join):

```
count = 2;
Fork L₁;
    a = x + y;    ← concurrent
    goto L₂;
L₁: b = z + 1;   ← concurrent
L₂: join count;
    c = a - b;
    w = c + 1;
```

---

## Parbegin / Parend (Concurrent Statement)

A higher-level construct by Dijkstra. All statements enclosed between `parbegin` and `parend` execute **concurrently**:

```
S₀;
parbegin
    S₁;
    S₂;
    ...
    Sₙ;
parend;
Sₙ₊₁;
```

**Example (our four statements):**

```
parbegin
    a = x + y;   ← concurrent
    b = z + 1;   ← concurrent
parend;
c = a - b;       ← not concurrent (sequential)
w = c + 1;
```

---

## Process Synchronization

### The Problem — Bounded Buffer with Counter

Adding a `counter` variable to the producer-consumer buffer (incremented when item added, decremented when item removed) seems logical but introduces a **race condition**.

```c
// Producer                    // Consumer
counter = counter + 1;         counter = counter - 1;
// Implemented as:             // Implemented as:
register1 = counter            register2 = counter
register1 = register1 + 1     register2 = register2 - 1
counter = register1            counter = register2
```

If both execute **concurrently**, execution interleaving can give wrong results:

```
S0: producer: register1 = counter        {register1 = 5}
S1: producer: register1 = register1+1   {register1 = 6}
S2: consumer: register2 = counter        {register2 = 5}
S3: consumer: register2 = register2-1   {register2 = 4}
S4: producer: counter = register1        {count = 6}
S5: consumer: counter = register2        {count = 4}  ← WRONG!
```

> Concurrent access to shared data may result in **data inconsistency**. Shared data must be accessed **atomically**.

---

## The Critical Section Problem

**Race Condition**: when several processes access and manipulate shared data concurrently, and the outcome depends on the particular order of access.

**Critical Section**: the code segment in which the shared data is accessed.

**Problem**: ensure that when one process is executing in its critical section, **no other process is allowed to execute in its critical section**.

### Structure of Process Pᵢ

```
repeat
    [entry section]      ← condition to enter critical section
    critical section
    [exit section]       ← what to do after leaving critical section
    remainder section
until false;
```

### Solution Requirements

1. **Mutual Exclusion**: if Pᵢ is in its critical section, no other process can be in theirs. One process at a time.
2. **Progress**: if no process is in the critical section and some want to enter, the selection cannot be postponed indefinitely.
3. **Bounded Waiting**: there must be a bound on the number of times other processes can enter the critical section after a process has made a request. No process waits forever.

### Types of Solutions

- **Software Solutions** (Programming): algorithms relying only on positive processing speed; use busy waiting.
- **Hardware Solutions**: rely on special machine instructions (system calls).
- **OS Solutions**: ready functions/data structures to support the programmer.

---

## Software Solutions

### Algorithm 1 — Turn Variable

```c
int turn;  // 0 or 1; if turn == i, Pᵢ can enter its critical section

// Process Pᵢ                // Process Pⱼ
do {                          do {
  while (turn != i)             while (turn != j)
    /* busy wait */;              /* busy wait */;
  critical section;             critical section;
  turn = j;                     turn = i;
  remainder section;            remainder section;
} while (true)                } while (true)
```

- **Mutual Exclusion**: ✓
- **Bounded Waiting**: ✓ (each waits at most 1 turn)
- **Progress**: ✗ — strict alternation; if P₀ finishes fast and P₁ is slow, P₀ must wait even if critical section is free.

### Algorithm 2 — Flag Array

```c
boolean flag[2];  // flag[i] = true means Pᵢ is ready to enter

// Process Pᵢ
do {
  flag[i] = true;
  while (flag[j]) /* do nothing */;
  critical section
  flag[i] = false;
  remainder section
} while (true)
```

- **Does NOT work**: both can set their flags to true simultaneously → **infinite loop** ("After you." "No, after you." "I insist." …)

### Algorithm 3 — Peterson's Solution (Combined)

Combines `turn` and `flag` from Algorithms 1 & 2:

```c
int turn;
boolean flag[2];  // flag[0] = flag[1] = false

// Process P₀                      // Process P₁
do {                                do {
  flag[0] = true;                     flag[1] = true;
  turn = 1;                           turn = 0;
  while (flag[1] && turn==1)          while (flag[0] && turn==0)
    /* do nothing */;                   /* do nothing */;
  critical section                    critical section
  flag[0] = false;                    flag[1] = false;
  remainder section                   remainder section
} while (true)                      } while (true)
```

- **Meets all three requirements** — solves the critical section problem for **two processes**.
- `flag[i]` says "I want to enter the critical section."
- `turn` resolves conflicts when both are ready — both give up their turn, so one will win.

### Bakery Algorithm — Generalization for n Processes

Each process takes a number. The process with the lowest number gets service next (like a bakery).

```c
boolean choosing[n];  // initialized to false
int number[n];        // initialized to 0

do {
    choosing[i] = true;
    number[i] = max(number[0], ..., number[n-1]) + 1;
    choosing[i] = false;
    for (j = 0; j < n; j++) {
        while (choosing[j]) /* do nothing */;
        while ((number[j] != 0) && (number[j],j) < (number[i],i))
            /* do nothing */;
    }
    critical section
    number[i] = 0;
    remainder section
} while (true)
```

- `(a,b) < (c,d)` if `a < c`, or if `a == c` and `b < d` (lexicographic order).
- Meets all three requirements for **n processes**.

### Drawbacks of Software Solutions

- Complicated to program.
- **Busy waiting** — wastes CPU cycles.
- Better to **block** waiting processes (just like I/O waiting).

---

## Hardware Solutions

### Disable Interrupts

On a **uni-processor**: disable interrupts during the critical section.

```
Repeat
    disable interrupts
    critical section
    enable interrupts
    remainder section
Forever
```

- Works on single-processor only.
- Does **not** work on multiprocessors.
- During critical section, multiprogramming is not utilized → **performance penalty**.

### Test-and-Set

An atomic hardware instruction that tests and modifies a word in a single operation:

```c
boolean Test_and_Set(Boolean &target) {
    boolean test = target;
    target = true;
    return test;
}

// Shared data:
boolean lock = false;

// Process Pᵢ
do {
    while (Test_and_Set(lock)) /* do nothing */;
    critical section
    lock = false;
    remainder section
} while (true)
```

> Must be executed **atomically** by hardware.

---

## Operating System Solution — Semaphores

A **semaphore** S is an integer variable accessed only via two **atomic** (indivisible) operations:

```
wait(S):   while (S <= 0) /* do nothing */;
           S = S - 1;

signal(S): S = S + 1;
```

- `wait` → **closes** the critical section (checks if it's empty).
- `signal` → **opens** the critical section.

> **Note**: `wait` and `signal` must be executed **atomically**.

**Usage for mutual exclusion:**

```
mutex: semaphore = 1;
Repeat
    wait(mutex);
    critical section
    signal(mutex);
    remainder section
Forever
```

**Problem with basic semaphore**: busy waiting.

### Semaphore Implementation (No Busy Waiting)

Define semaphore as a struct with a waiting list:

```c
struct semaphore {
    int value;
    List *L;  // list of waiting processes
}

wait(S):
    S.value = S.value - 1;
    if (S.value < 0) {
        add this process to S.L;
        block;
    }

signal(S):
    S.value = S.value + 1;
    if (S.value <= 0) {
        remove a process P from S.L;
        wakeup(P);
    }
```

- **block**: suspends the process.
- **wakeup(P)**: resumes the blocked process P.

---

## Classical Problems of Synchronization

### 1. Bounded Buffer Problem

```c
semaphore full = 0;    // counting semaphore
semaphore empty = n;   // counting semaphore
semaphore mutex = 1;   // binary semaphore (mutual exclusion)

// Producer                   // Consumer
do {                           do {
  produce item in nextp;         wait(full);
  wait(empty);  // buffer full?  wait(mutex);
  wait(mutex);  // counter       remove item from buffer to nextc;
  add nextp to buffer;           signal(mutex);
  signal(mutex);                 signal(empty);
  signal(full);                  consume item in nextc;
} while (true)                 } while (true)
```

### 2. Readers-Writers Problem

Multiple readers can read simultaneously, but a writer needs exclusive access.

```c
semaphore mutex = 1;
semaphore wrt = 1;
int readcount = 0;

// Writer                    // Reader
wait(wrt);                   wait(mutex);
  writing is performed;      readcount = readcount + 1;
signal(wrt);                 if (readcount == 1) wait(wrt);
                             signal(mutex);
                             reading is performed;
                             wait(mutex);
                             readcount = readcount - 1;
                             if (readcount == 0) signal(wrt);
                             signal(mutex);
```

### 3. Dining Philosophers Problem

5 philosophers sit at a circular table. Each needs 2 chopsticks (semaphores) to eat.

```c
semaphore chopstick[5];
chopstick[] = 1;  // all available

// Philosopher i:
do {
    wait(chopstick[i]);
    wait(chopstick[(i+1) mod 5]);
    eat;
    signal(chopstick[i]);
    signal(chopstick[(i+1) mod 5]);
    think;
} while (true)
```

> **Problems**: (1) **Deadlock** — all philosophers pick up their left chopstick simultaneously. (2) **Starvation**.
>
> **Solution**: philosophers pick up chopsticks in order `min(i, (i+1)%5)` first, then `max(i, (i+1)%5)`.
