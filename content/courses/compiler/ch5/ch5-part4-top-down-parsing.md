---
title: "Chapter 5, Part 4 – Top-Down Parsing"
description: "FIRST and FOLLOW sets, Recursive Descent Parsing, LL(1) Parsing, and problems with top-down parsing."
date: 2026-01-01
toc: true
---

## Overview

In top-down parsing, the parser builds the derivation tree from the **root (S) down to the leaves** using **leftmost derivation**.

**Major Problem:** The parser does not know which production rule to apply at each derivation step — it can be non-deterministic for general CFGs.

We cannot build deterministic top-down parsers for all CFGs. However, for certain restricted classes, we can. Two main deterministic top-down techniques exist:

1. **Recursive Descent Parsing**
2. **LL(1) Parsing**

Both depend heavily on **FIRST()** and **FOLLOW()** sets.

---

## Foundations: FIRST() and FOLLOW()

### The FIRST() Function

Given a string α ∈ V*, `FIRST(α)` is the set of all **terminals** that may begin strings derived from α. If `α →* λ`, then λ ∈ FIRST(α) as well.

```
If:  α →* cBx
     α →* ayD
     α →* ab
     α →  ddd

Then:  FIRST(α) = {c, a, d}

If also: α →* λ
Then:    FIRST(α) = {c, a, d, λ}
```

### The FOLLOW() Function

`FOLLOW(A)` is defined **only for non-terminals**. It is the set of all **terminals** that may appear **immediately after** A in any derivation from S:

```
FOLLOW(A) = { a | S →* uAβ, where a ∈ FIRST(β) }
```

```
If:  S →* aaXdd
     S →* Xa
     S →* BXc

Then:  FOLLOW(X) = {d, a, c}
```

### Rules to Compute FIRST() and FOLLOW()

1. `FIRST(λ) = {λ}`
2. `FIRST(a) = {a}` — where a is a terminal
3. `FIRST(aα) = {a}`
4. `FIRST(XY) = FIRST( FIRST(X) · FIRST(Y) )`
5. Given the production `A → αXβ`:
   - If β ≠ λ: `FIRST(β) ⊆ FOLLOW(X)`
   - If β = λ: `FOLLOW(A) ⊆ FOLLOW(X)`

> **Why Rule 5 works:**
> Consider the derivation `S →* uAG → uαXβG`.
>
> - If β ≠ λ: X is directly followed by β, so FIRST(β) ⊆ FOLLOW(X).
> - If β = λ: the production becomes A → αX, so X is followed by whatever follows A — therefore FOLLOW(A) ⊆ FOLLOW(X).

> **Notes:**
>
> - FIRST() and FOLLOW() sets contain **terminals only** (λ may appear in FIRST but **never** in FOLLOW).
> - Compute FIRST() **bottom-up**; compute FOLLOW() **top-down**.
> - When computing FOLLOW(X), search for X on the right-hand side of any production.
> - **Shortcut:** If λ ∉ FIRST(A), then `FIRST(AB) = FIRST(A)` — B can be ignored entirely.

---

### Augmented Grammars

Given grammar G = (V_N, V_T, S, P), the **augmented grammar** G' = (V'\_N, V'\_T, S', P') is obtained as:

1. V'\_N = V_N ∪ {S'}
2. V'\_T = V_T ∪ {$}  — where `$` is the **stop symbol**
3. S' = new starting symbol
4. P' = P ∪ {S' → S$}

**Example** — augmenting the arithmetic grammar:

```
Original:   E → E + T | T     T → T * F | F     F → (E) | a

Augmented:  E' → E$
            E  → E + T | T
            T  → T * F | F
            F  → (E) | a
```

The augmented grammar creates a FOLLOW set for E (FOLLOW(E) = {$, +, )}) by providing a clear start symbol E' and stop symbol $.

---

### Extended BNF and Syntax Diagrams

Standard BNF can be verbose for repetitive patterns. **Extended BNF (EBNF)** introduces:

- `[x]` — x appears **zero or one time** (optional)
- `{x}` or `(x)*` — x appears **zero or more times**
- `(x)+` — x appears **one or more times**

**Example** — rewriting the arithmetic grammar:

```
Standard BNF:        EBNF:
E → E + T | T   →   E → T {+T}    or    E → T (+T)*
T → T * F | F   →   T → F {*F}          T → F (*F)*
F → (E) | a     →   F → (E) | a         F → (E) | a
```

**Syntax Diagrams** are a visual representation of EBNF. A **rectangle** represents a non-terminal; an **oval** represents a terminal. Loops allow repetition; branching paths represent alternatives.

For `E → T (+T)*`:

```
─→─ [T] ─→──────────────→─
          ↑              |
          └──── [+] [T] ←┘
```

For `F → (E) | a`:

```
─→─ [(] ─ [E] ─ [)] ─→─
  ↘                  ↗
    ─────── [a] ──────
```

> **Note:** `{ }`, `[ ]`, and `( )*` are **meta-symbols** describing grammar structure — they are not terminals in the language itself.

---

### Worked Examples — FIRST/FOLLOW

**Example 1:**

```
S' → S$
S  → AB
A  → a | λ
B  → b | λ
```

```
FIRST(A) = {a, λ}
FIRST(B) = {b, λ}
FIRST(S) = FIRST(AB) = FIRST({a,λ}·{b,λ}) = {a, b, λ}
FIRST(S') = FIRST(S$) = {a, b, $}

FOLLOW(S)  = {$}
FOLLOW(A)  = {b, $}   (FIRST(B)={b}; if B→λ then FOLLOW(S)={$})
FOLLOW(B)  = {$}      (B is at the end, so FOLLOW(S)={$})
```

**Example 2:**

```
S' → S$
S  → aAcb | Abc
A  → b | c | λ
```

```
FIRST(A) = {b, c, λ}
FIRST(S) = {a} ∪ FIRST(Abc) = {a} ∪ {b, c} = {a, b, c}
FIRST(S') = {a, b, c}

FOLLOW(S) = {$}
FOLLOW(A) = {c, b}
```

**Example 3:**

```
G → E$
E → E + T | T
T → T * F | F
F → (E) | a
```

```
FIRST(F) = {(, a}

FIRST(T):  T →* F*F*...*F, so FIRST(T) = FIRST(F) = {(, a}
           (since λ ∉ FIRST(F), we stop at F)

FIRST(E):  E →* T+T+...+T, so FIRST(E) = FIRST(T) = {(, a}

FIRST(G) = FIRST(E$) = FIRST(E) = {(, a}

FOLLOW(E) = {$, +, )}
FOLLOW(T) = FOLLOW(E) ∪ {*} = {$, +, *, )}
FOLLOW(F) = FOLLOW(T) = {$, +, *, )}
```

---

## Recursive Descent Parsing

**Recursive Descent Parsing** is a top-down technique where each non-terminal in the grammar is implemented as a **recursive function**. The parser processes tokens left-to-right, using FIRST and FOLLOW sets to choose the correct production.

The parser maintains a pointer to the current token. `get_token()` advances the pointer to the next token.

```
while  (  x  >=  100  )  $
  ↑
current token          after get_token() →
```

### Translation Rules

**Rule 1 — terminal `a`:**

```javascript
if (token == a) get_token();
else report_error();
```

**Rule 2 — concatenation `X → α₁α₂…αₙ`:**

```javascript
Code(X) {
    Code(α₁);
    Code(α₂);
    ...
    Code(αₙ);
}
```

**Rule 3 — alternatives `X → α₁ | α₂ | … | αₙ` (no λ):**

```javascript
Code(X) {
    if      (token in FIRST(α₁)) Code(α₁);
    else if (token in FIRST(α₂)) Code(α₂);
    ...
    else if (token in FIRST(αₙ)) Code(αₙ);
    else report_error();
}
```

**Rule 4 — alternatives with λ (say αₙ = λ):**

```javascript
Code(X) {
    if      (token in FIRST(α₁))   Code(α₁);
    else if (token in FIRST(α₂))   Code(α₂);
    ...
    else if (token in FIRST(αₙ₋₁)) Code(αₙ₋₁);
    else if (token NOT in FOLLOW(X))
        report_error();
    // else: apply λ production (do nothing)
}
```

**Rule 5 — repetition `X → α*` (EBNF):**

```javascript
Code(X) {
    while (token in FIRST(α))
        Code(α);
}
```

> **Notes:**
>
> - Every non-terminal has a corresponding function.
> - S' (augmented start) is represented by `main()`.
> - `get_token()` is called **only once**, at the start of `main()`.
> - EBNF operators map directly to code constructs: `()*` → `while`, `[]` → `if`, `|` → `if-else`. This makes the translation from grammar to code straightforward.

---

### Worked Examples — Recursive Descent

**Example 1:**

```
G → E$
E → T (+T)*
T → F (*F)*
F → (E) | a
```

```javascript
main() {
    get_token();
    E();
    if (current_token == '$') success();
    else report_error();
}

function E() {        // E → T (+T)*  [Rules 2 & 5]
    T();
    while (current_token == '+') {
        get_token();
        T();
    }
}

function T() {        // T → F (*F)*  [Rules 2 & 5]
    F();
    while (current_token == '*') {
        get_token();
        F();
    }
}

function F() {        // F → (E) | a  [Rule 3]
    if (current_token == '(') {
        get_token();
        E();
        if (current_token == ')') get_token();
        else report_error();
    }
    else if (current_token == 'a') get_token();
    else report_error();
}
```

**Example 2:**

```
Program → body .
body    → Begin stmt (; stmt)* End
stmt    → Read | Write | body | λ
```

Valid programs in this language:

```
Begin Read; Write; Read; End.
Begin Read; End.
Begin Read; Begin Read; Write; End. Write; End.
Begin ; ; ; End.
```

```javascript
main() {
    get_token();
    body();
    if (token != '.') report_error();
    else success();
}

function body() {
    if (token == Begin) {
        get_token();
        stmt();
        while (token == ';') {
            get_token();
            stmt();
        }
        if (token == End) get_token();
        else report_error();
    }
    else report_error();
}

function stmt() {
    if      (token == Read)  get_token();
    else if (token == Write) get_token();
    else if (token == Begin) body();   // FIRST(body) = {Begin}
    else if (!(token == ';' || token == End))
        report_error();
    // else: λ production (empty statement) — FOLLOW(stmt) = {; , End}
}
```

---

## LL(1) Parsing

### What is LL(k)?

An **LL(k) parser** is a top-down parser that uses **k lookahead symbols** to make parsing decisions:

- **L** — scans input from **L**eft to right
- **L** — produces a **L**eftmost derivation
- **(k)** — uses k tokens of lookahead

In practice, **k = 1** is sufficient for most programming languages. The larger k, the more powerful the parser — but also more complex to implement.

> **Note:** Ambiguity only occurs when a non-terminal has **multiple** alternative productions. If each non-terminal has exactly one production, parsing is already deterministic.

### Definition of LL(1)

LL(1) parsing is **table-driven**. The parsing table directs the parser to the correct production based on: (1) the current non-terminal, and (2) the current input token.

**A grammar is LL(1) if, for every non-terminal A with alternatives `A → α₁ | α₂ | … | αₙ`:**

1. **Disjoint FIRST sets** (when no λ productions exist):
   `FIRST(αᵢ) ∩ FIRST(αⱼ) = ∅` for all i ≠ j.

2. **FIRST and FOLLOW are disjoint** (when one alternative is λ, say αₙ = λ):
   `FIRST(αᵢ) ∩ FOLLOW(A) = ∅` for all i < n.

> **Intuition:** Condition 1 ensures no two alternatives can begin with the same token. Condition 2 ensures that when λ is an option, what can start the other alternatives doesn't overlap with what can follow A. Together, one lookahead token is always enough.

---

### Building the LL(1) Parsing Table

**Algorithm:** For each production `A → α` in the grammar:

- For every `a ∈ FIRST(α)`: add `A → α` to `T[A, a]`.
- If `λ ∈ FIRST(α)`: for every `b ∈ FOLLOW(A)`, add `A → α` to `T[A, b]`.

All remaining entries are **error** entries.

> If any table cell gets more than one entry, the grammar is **not LL(1)**.

---

### Worked Examples — LL(1)

**Example 1 — Floating Point Numbers:**

```
V  → SR$    (1)
S  → +      (2)
S  → −      (3)
S  → λ      (4)
R  → dN.N   (5)
R  → .dN    (6)
N  → dN     (7)
N  → λ      (8)
```

FIRST and FOLLOW sets:

```
FIRST(S) = {+, −, λ}      FOLLOW(S) = {d, .}
FIRST(R) = {d, .}         FOLLOW(R) = {$}
FIRST(N) = {d, λ}         FOLLOW(N) = {., $}
```

Parsing Table:

| V_N \ V_T | `+` | `−` | `d` | `.` | `$` |
| --------- | --- | --- | --- | --- | --- |
| V         | 1   | 1   | 1   | 1   | E   |
| S         | 2   | 3   | 4   | 4   | E   |
| R         | E   | E   | 5   | 6   | E   |
| N         | E   | E   | 7   | 8   | 8   |

No conflicts → **LL(1) grammar**. L(G) = all floating-point numbers.

**Parsing trace for `−dd.d$`:**

| Stack | Remaining Input | Action        |
| ----- | --------------- | ------------- |
| V     | −dd.d$          | Production 1  |
| SR$   | −dd.d$          | Production 3  |
| −R$   | −dd.d$          | Pop & advance |
| R$    | dd.d$           | Production 5  |
| dN.N$ | dd.d$           | Pop & advance |
| N.N$  | d.d$            | Production 7  |
| dN.N$ | d.d$            | Pop & advance |
| N.N$  | .d$             | Production 8  |
| .N$   | .d$             | Pop & advance |
| N$    | d$              | Production 7  |
| dN$   | d$              | Pop & advance |
| N$    | $               | Production 8  |
| $     | $               | Pop & advance |
| λ     | λ               | **Accept**    |

> If at any point the stack top and the current input are **two different terminals**, the parser throws a syntax error.

**Example 2 — Block-Statement Language:**

```
program   → block $         (1)
block     → {decls stmts}   (2)
decls     → D; decls        (3) | λ  (4)
stmts     → statement; stmts (5) | λ (6)
statement → if(7) | while(8) | ass(9) | scan(10) | print(11) | block(12) | λ(13)
```

FIRST and FOLLOW sets:

```
FIRST(program)   = {{}
FIRST(block)     = {{}        FOLLOW(block)    = {;, $}
FIRST(decls)     = {D, λ}     FOLLOW(decls)    = {if, while, ass, scan, print, {, }, ;}
FIRST(stmts)     = {if, while, ass, scan, print, {, ;, λ}   FOLLOW(stmts) = {}}
FIRST(statement) = {if, while, ass, scan, print, {, ;, λ}   FOLLOW(statement) = {;}
```

Parsing Table:

| V_N \ V_T | if  | while | ass | scan | print | `{` | `}` | D   | `;` | `$` |
| --------- | --- | ----- | --- | ---- | ----- | --- | --- | --- | --- | --- |
| program   |     |       |     |      |       | 1   |     |     |     |     |
| block     |     |       |     |      |       | 2   |     |     |     |     |
| decls     | 4   | 4     | 4   | 4    | 4     | 4   | 4   | 3   | 4   |     |
| stmts     | 5   | 5     | 5   | 5    | 5     | 5   | 6   |     | 5   |     |
| statement | 7   | 8     | 9   | 10   | 11    | 2   |     |     |     | 13  |

**Example 3 — Dangling Else (LL(1) Conflict and Fix):**

```
S' → S$    (1)
S  → iCSE  (2)
E  → eS    (3) | λ  (4)
S  → a     (5)
C  → c     (6)
```

Initial table has a **conflict** in state E on token `e` (both productions 3 and 4 apply). **Fix:** remove production 4 from the conflict entry — prefer to shift (match `else` with the nearest `if`):

| V_N \ V_T | `i` | `a` | `e` | `c` | `$` |
| --------- | --- | --- | --- | --- | --- |
| S'        | 1   | 1   |     |     |     |
| S         | 2   | 5   |     |     |     |
| E         |     |     | 3   |     |     |
| C         |     |     |     | 6   |     |

> **Note:** If a grammar is LL(1), it is unambiguous. However, the converse is not necessarily true.

---

## Problems with Top-Down Parsing

### Left Recursion

A grammar is **left-recursive** if it has a production of the form `A → Aα`. This causes **infinite recursion** in recursive descent parsing — the function calls itself immediately without consuming any input:

```javascript
// Translating E → E + T | T directly:
function E() {
  E(); // Immediate infinite recursion → stack overflow!
  match("+");
  T();
}
```

Right-recursive grammars don't have this issue — `α` is processed first, consuming tokens and ensuring the recursion terminates.

> **Why left recursion violates LL(1):** For `A → Aα | β`, every derivation must eventually use `A → β` to terminate. So FIRST(Aα) = FIRST(A) = FIRST(β), meaning `FIRST(Aα) ∩ FIRST(β) ≠ ∅` — a direct LL(1) violation creating a conflict in the parsing table.

**Transformation — Eliminating Left Recursion:**

Given:

```
A → Aα₁ | Aα₂ | ... | Aαₙ | β₁ | β₂ | ... | βₘ
```

Introduce a new non-terminal A' and rewrite as:

```
A  → β₁A' | β₂A' | ... | βₘA'
A' → α₁A' | α₂A' | ... | αₙA' | λ
```

**Simple example:**

```
Before:             After:
A → Ab  (L(G)=ab*) A  → aA'
A → a               A' → bA' | λ
```

**Applied to the arithmetic grammar:**

```
Before:                    After:
E → E + T | T              E  → TE'
T → T * F | F              E' → +TE' | λ
F → (E) | a                T  → FT'
                            T' → *FT' | λ
                            F  → (E) | a
```

This grammar is unambiguous, handles precedence and associativity correctly, and is ready to be used for parser construction.

---

### The Dangling Else Problem

The grammar:

```
if-stmt → IF condition stmt
if-stmt → IF condition stmt ELSE stmt
```

is **ambiguous** — the nested statement `IF C IF C S ELSE S` has two valid derivation trees (ELSE could belong to either IF).

**Solution 1 — Add a delimiter** (e.g. `ENDIF`):

```
if-stmt → IF condition stmt ENDIF
if-stmt → IF condition stmt ELSE stmt ENDIF
```

The grammar becomes unambiguous, but requires more keywords and produces less readable code.

**Solution 2 — Shift preference** (most common in practice):

When the parser sees ELSE, it **shifts** — attaching ELSE to the **nearest unmatched IF**. This matches the convention of C, Java, Python, and most real languages.

- **Advantages:** matches programmer intuition; no extra syntax required.
- **Disadvantages:** grammar remains technically ambiguous; relies on parser behavior rather than grammar rules.

---

### Left Factoring

When two productions share a common prefix:

```
A → αβ
A → αγ
```

This is not LL(1) — both alternatives start with α, creating a conflict. **Fix:** introduce a new non-terminal to factor out the common prefix:

```
A → αB
B → β | γ
```

**Applied to if…else:**

```
Before:
  if-stmt → IF condition stmt
  if-stmt → IF condition stmt ELSE stmt

After:
  if-stmt   → IF condition stmt else-part
  else-part → ELSE stmt | λ
```

Left factoring reduces the number of choices in the parsing table. However, it does **not** resolve the underlying ambiguity of the dangling else — the shift-preference rule is still needed alongside it.
