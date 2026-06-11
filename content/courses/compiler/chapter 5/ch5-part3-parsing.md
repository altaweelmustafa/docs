---
title: "Chapter 5, Part 3 – Parsing"
description: "Top-down and bottom-up parsing techniques, and how to detect and resolve grammar ambiguity."
date: 2026-01-01
---

## Chapter 5, Part 3 – Parsing

*posted on 2026 Jan 01*

**Contents**

- [What is Parsing?](#what-is-parsing)
- [Parsing Techniques](#parsing-techniques)
- [Ambiguity](#ambiguity)

---

## What is Parsing?

**Parsing** is the process of analyzing a sequence of tokens to determine its grammatical structure with respect to a given formal grammar.

Given a grammar G = (V_N, V_T, S, P) and an input string w, the parser answers: **Is w ∈ L(G)?**

The parser's job:

1. **Accept or Reject** — determine if the input string is a valid sentence in the language.
2. **Report Errors** — if invalid, identify and report syntax errors.
3. **Build Structure** — if valid, construct a parse tree representing the syntactic structure.

> **Note:** Parsers in general do not construct explicit trees in practice. However, conceptually, parsing can be understood as attempting to build a derivation that shows how the input derives from the start symbol.

---

## Parsing Techniques

There are 2 main parsing techniques used by a compiler.

### Top-Down Parsing

The parser builds the derivation tree **from the root (S) down to the leaves (sentence)** — it tries to derive the sentence using **leftmost derivation**.

```
S  →*  program    (leftmost derivation)
```

**Example** — checking if `+dd.d$` is in the language of the grammar `V → SR$`, `S → + | − | λ`, `R → .dN | dN.N`, `N → dN | λ`:

```
V → SR$ → +R$ → +dN.N$ → +ddN.N$ → +dd.N$ → +dd.dN$ → +dd.d$  ✓
```

Since we ended with all terminals, the sentence is valid. The parse tree for this derivation:

```
         V
        / \
       S   R            $
       |   |\ \
       +   d N . N
             |     \
             d N  d N
               |      \
               λ       λ
```

> **Major Problem:** The parser does not know which production to select at each derivation step — this is the central challenge of top-down parsing. We will learn how to solve this with Recursive Descent and LL(1) parsing.

### Bottom-Up Parsing

The parser builds the derivation tree **from the leaves (sentence) up to the root (S)** — it applies **reduction** steps (the opposite of derivation) until the starting symbol is reached. The resulting tree built from leaves to root is called a **B-Tree**.

```
Sentence  →*  S    (reduction)
```

> **Note:** The string λ is present everywhere implicitly, and can be inserted wherever needed.

**Example** — reducing `+dd.d$` back to V:

```
+dd.d$ → +ddλ.d$ → +ddN.d$ → +dN.d$ → +dN.dλ$
       → +dN.dN$ → +dN.N$ → +R$ → SR$ → V  ✓
```

The sentence is in the grammar. However, taking the wrong reduction path leads to a deadlock:

```
+dd.d$ → +ddλ.d$ → +ddN.d$ → +dN.d$ → +dN.dλ$
       → +dN.dN$ → +dNR$ → +NR$ → SNR$ → Deadlock
```

> **Major Problem:** Which substring should be selected for reduction at each step? This is the central challenge of bottom-up parsing — it is solved using **handles** and LR parsing techniques.

---

## Ambiguity

Consider the grammar:

```
num → num d
num → d
```

The sentence `dddd` has exactly one derivation tree — there is only one way to derive it. This grammar is **unambiguous**.

**Definition:** A grammar G is **ambiguous** if there is **at least one sentence with more than one derivation tree** — that is, there is more than one way to derive the sentence. This makes the parsing algorithm non-deterministic.

**Example** — the grammar:

```
E → E + E
E → E * E
E → (E) | a
```

The sentence `a + a * a` has **two** derivation trees:

*Tree 1 — represents `a + (a * a)` (multiplication first — mathematically correct):*

```
        E
       /|\
      E + E
      |   /|\
      a  E * E
         |   |
         a   a
```

*Tree 2 — represents `(a + a) * a` (addition first — incorrect by convention):*

```
        E
       /|\
      E * E
     /|\  |
    E + E a
    |   |
    a   a
```

Because both trees are valid derivations of the same sentence, this grammar is **ambiguous**. The grammar does not enforce operator precedence.

### Attempt 1 — Separating Operations into Levels

To enforce precedence (multiplication before addition):

```
E → E + E | T
T → T * T | F
F → (E) | a
```

This solves the precedence problem — `a + a * a` now correctly represents `a + (a * a)`. But the sentence `a + a + a` still has more than one derivation tree, so **associativity** is not yet resolved.

### Solution — Left-Associative Grammar

```
E → E + T | T
T → T * F | F
F → (E) | a
```

Derivation tree for `a + a + a` — only one possible tree, representing `(a + a) + a`:

```
        E
       /|\
      E + T
     /|\   \
    E + T   F
    |   |   |
    T   F   a
    |   |
    F   a
    |
    a
```

Derivation tree for `a + a * a` — only one possible tree, representing `a + (a * a)`:

```
        E
       /|\
      E + T
      |   /|\
      T  T * F
      |  |   |
      F  F   a
      |  |
      a  a
```

This grammar solves all three major problems:
- **Ambiguity** — only one derivation tree per sentence.
- **Precedence** — multiplication has higher precedence than addition (`*` is deeper in the tree).
- **Associativity** — operations of the same level associate left-to-right.

### Alternative — Right-Associative Grammar

```
E → T + E | T
T → F * T | F
F → (E) | a
```

This grammar is also unambiguous, but interprets `a + a + a` as `a + (a + a)` — right-to-left. Since standard mathematics and most programming languages associate left-to-right, **the left-associative grammar is the correct solution** for arithmetic expressions.

[*Chapter 5, Part 4 – Top-Down Parsing*](../ch5-part4-top-down-parsing/)
