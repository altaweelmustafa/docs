---
title: "Chapter 4, Part 4 – Creating a NDFSA from a Regular Expression"
date: 2026-01-01
weight: 4
toc: true
tags: ["compiler", "NDFSA", "DFSA", "regular-expressions", "construction"]
description: "How to construct an NDFSA from a regular expression using construction rules, then convert it to a DFSA."
---

## FSM for Basic Tokens

Simple finite state machines for basic tokens:

**Names** — `Letter(Letter|digit)* = L(L|d)*`

```
S --L--> F
         |
      L,d (loop)
```

**Integers** — `[+|−]digit(digit)* = [+|−]d+`

```
S --+/--> A --d--> F
               |
             d (loop)
```

Both machines are deterministic: no λ-transitions, no state has multiple transitions on the same input.

---

## Construction Methodology

To construct a NDFSA from a regular expression:

1. Decompose the expression into **primitive components**:
   - For the empty string λ: `X --λ--> Y`
   - For a symbol `a ∈ V`: `X --a--> Y`

2. Apply **construction rules** to combine primitives into complex expressions.

Given N1 and N2 are FSAs accepting R1 and R2 respectively, each with a start state and a final state.

---

## Construction Rules

### Concatenation: R1R2

Merge the final state of N1 with the start state of N2. The merged state is **no longer final**.

```
N1: S1 ... [F1/S2] N2: ... F2
```

### Union: R1|R2

Create a new start state with λ-transitions to both N1 and N2. Connect both final states to a new final state via λ.

```
       λ --> N1 --> λ
S -->                  --> F
       λ --> N2 --> λ
```

### Kleene Star: R*

Allow zero or more repetitions. Add a λ-bypass from start to final, and a λ-loop from final back to start.

```
S --λ--> N --λ--> F
 \________________/
         λ (bypass)
```

### Positive Closure: R+

Same as R* but **without** the λ-bypass — ensuring at least one execution.

```
S --> N --> F
      ^_____|
          λ
```

---

## Complete Example: L(L|d)*

**Step 1: Remove λ transitions**

Initial state table:

| State | L | d | λ |
|-------|---|---|---|
| 1 | 2 | | |
| 2 | | | 3,9,4,6 |
| 3 | | | 4,6 |
| 4 | 5 | | |
| 5 | | | 8,3,9,4,6 |
| 6 | | 7 | |
| 7 | | | 8,3,9,4,6 |
| 8 | | | 3,9,4,6 |
| 9 | | | |

After removing λ column (marking reachable final states):

| State | L | d | Marked |
|-------|---|---|--------|
| 1 | 2 | | ✓ |
| 2 | 5 | 7 | ✓ |
| 5 | 5 | 7 | ✓ |
| 7 | 5 | 7 | ✓ |

States 3, 4, 6, 8, 9 are inaccessible — removed.

**Step 2: Remove Non-Determinism**

All remaining states are already deterministic.

**Step 3: Remove Inaccessible States**

Already done above.

**Step 4: Merge Equivalent States**

Feasible pairs table:

| Feasible pairs | L | d | Marked |
|----------------|---|---|--------|
| (2,5) | (5,5) | (7,7) | |
| (2,7) | (5,5) | (7,7) | |
| (5,7) | (5,5) | (7,7) | |

No pairs marked → `2 ≡ 5 ≡ 7 → 2`

**Final state table:**

| State | L | d |
|-------|---|---|
| 1 | 2 | |
| 2 | 2 | 2 |

The final DFSA has just 2 states and accepts the language `L(L|d)*` — identifiers starting with a letter followed by any letters or digits.
