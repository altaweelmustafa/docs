---
title: "Chapter 1, Part 2 – Syntax & Semantics"
date: 2026-06-19
weight: 2
toc: true
tags: ["programming-languages", "syntax", "semantics", "tokens"]
description: "How CFG defines syntax, and how the scanner/parser pipeline turns code into tokens and structures."
---

# Chapter 1 - Introduction and Background
## Part 2: Syntax & Semantics

---

## Syntax

Syntax is the grammar of the programming language. It describes structures like expressions, statements, and blocks.

Syntax is formally described using a **Context Free Grammar (CFG)**, a set of static algorithms and frameworks.

---

## Semantics

Semantics gives meaning to the syntax structures. It's much harder to pin down precisely than syntax. For example, the meaning of an if/else statement has to be implemented correctly so the compiler generates the right code.

There's no clean formal system for semantic analysis the way CFG works for syntax. There is a framework called **Syntax Directed Translation (SDT)** used to express semantic analysis.

### The Translation Pipeline

```
Code -> Scanner (Lexical Structure) -> Tokens -> Syntax Analyzer -> Object Code
```

The Scanner reads the source and groups characters into **Tokens**. The Syntax Analyzer then takes those tokens and tries to build valid syntax structures, moving to the next group of tokens once the current one checks out.

### Example

```c
if(x!=1){ n++; }
```

Tokens: `if`, `(`, `x`, `!=`, `1`, `)`, `{`, `n`, `++`, `;`, `}`

The Syntax Analyzer checks this piece by piece:
1. Is `if(x!=1)` valid?
2. Is `n++;` valid?
3. Is the whole if-statement valid?
