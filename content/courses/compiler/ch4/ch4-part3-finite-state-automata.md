---
title: "Chapter 4, Part 3 – Finite State Automata (FSA)"
date: 2026-01-01
weight: 3
toc: true
tags: ["compiler", "FSA", "NDFSA", "DFSA", "automata"]
description: "How finite state automata recognize tokens, and the two types: deterministic and non-deterministic."
---

The algorithm that recognizes strings of regular languages is called the **Finite State Automata (FSA)**, also known as the Finite State Machine or Finite State System. It contains:

1. A set of states.
2. Transitions between states.
3. An input string to be examined.

---

## FSA Example

Consider this FSA for floating point numbers, with states `Q = {S, A, B, G, H}`:

- **S** — Starting State
- **H** — Final (Halt) State
- Transitions: `{+, −, d, .}`

Tracking the input `"-511.32"` through the states:

```
S --> A --> B --> B --> B --> H --> H --> H
-     d     d     d     .     d     d
```

Since we end in the final state H, the string is **accepted**.

**Definition:** A string is accepted if after scanning the whole string we end up in a final state.

The language accepted by this machine is:

`L(M) = [+|−](d+. | .d+ | d+.d+) = [+|−](d+.d* | d*.d+)`

---

## Types of Finite State Automata

### Non-Deterministic FSA (NDFSA)

A FSA is **non-deterministic** if any of the following is true:

1. There are **λ-transitions** in the FSA.
2. There is **more than one transition** from the same state on the same input.

Non-determinism requires backtracking to implement — which is extremely slow and impractical in a compiler.

### Deterministic FSA (DFSA)

A FSA is **deterministic** if both of the following are true:

1. There are **no λ-transitions**.
2. There is **no more than one transition** from the same state on the same input.

**Only Deterministic FSAs are used in compilers.** Fortunately, any NDFSA can be transformed into an equivalent DFSA.

---

## Transformation of NDFSA to DFSA

The transformation consists of 4 steps:

1. Removal of λ transitions.
2. Removal of non-determinism.
3. Removal of inaccessible states.
4. Merging equivalent states.

Steps 3 and 4 are post-processing — at that point the machine is already deterministic.

### Example

Given the NDFSA for integers and floating point numbers, with `L(G) = [+|−](d+ | d+.d+ | d+. | .d+)`:

**Initial transition table:**

| State | + | - | . | d | λ |
|-------|---|---|---|---|---|
| S | A | A | | | A |
| A | | | | B,C | E |
| B | | | F | B | |
| C | | | | D | C |
| D | | | | D | F |
| E | | | | G | E |
| G | | | | H | |
| H | | | | H | F |
| F | | | | | |

---

### Step 1: Remove λ Transitions

For each state with a λ-transition, copy all transitions from the target state into the source state. Mark states that have a λ-transition to a final state as final. Delete the λ column.

**After removing λ transitions:**

| State | + | - | . | d |
|-------|---|---|---|---|
| S | A | A | G | B,C,E |
| A | | | G | B,C,E |
| B | | | | B |
| C | | | | D,C |
| D | | | | D |
| E | | | | G,E |
| G | | | | H |
| H | | | | H |
| F | | | | |

---

### Step 2: Remove Non-Determinism

Treat each set of states as a new single state. If any state in the set is a final state, mark the new state as final.

**After resolving all non-deterministic states:**

| State | + | - | . | d |
|-------|---|---|---|---|
| S | A | A | G | B,C,E |
| A | | | G | B,C,E |
| G | | | | H |
| H | | | | H |
| B,C,E | | | D,G | B,C,E |
| D,G | | | | D,H |
| D,H | | | | D,H |

---

### Step 3: Remove Inaccessible States

Start from S, mark all reachable states exhaustively. Delete unmarked states.

**After removal, renaming [B,C,E] → X, [D,G] → Y, [D,H] → Z:**

| State | + | - | . | d |
|-------|---|---|---|---|
| S | A | A | G | X |
| A | | | G | X |
| G | | | | H |
| H | | | | H |
| X | | | Y | X |
| Y | | | | Z |
| Z | | | | Z |

---

### Step 4: Merge Equivalent States

**Definition:** States p and q are equivalent if for every input string X, the machine accepts X starting from p if and only if it accepts X starting from q.

Separate final from non-final states:

- `Q − F = {S, A, G}`
- `F = {H, X, Y, Z}`

**Feasible pairs table:**

| Feasible pairs | + | - | . | d | Marked |
|----------------|---|---|---|---|--------|
| (H, Y) | | | | H,Z | |
| (H, Z) | | | | H,Z | |
| (Y, Z) | | | | Z,Z | |

No pairs are marked → `H ≡ Y ≡ Z → H`

**Final simplified table:**

| State | + | - | . | d |
|-------|---|---|---|---|
| S | A | A | G | X |
| A | | | G | X |
| G | | | | H |
| H | | | | H |
| X | | | H | X |

This is the **minimum state machine**, and it accepts the same language:

`L(G) = [+|−]{d+, d+.d+, d+., .d+}`
