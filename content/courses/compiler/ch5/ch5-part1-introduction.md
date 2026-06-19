---
title: "Chapter 5, Part 1 – Introduction to Syntax Analysis"
description: "What the parser does, BNF notation, and how the scanner connects to the parser."
date: 2026-01-01
---

## Chapter 5, Part 1 – Introduction to Syntax Analysis

*posted on 2026 Jan 01*

**Contents**

- [What is a Syntax Analyzer?](#what-is-a-syntax-analyzer)
- [Connecting the Scanner and Parser](#connecting-the-scanner-and-parser)

---

## What is a Syntax Analyzer?

A **Syntax Analyzer** (or **Parser**) is an algorithm that groups the set of tokens sent by the Scanner to form syntax structures such as Expressions, Statements, Blocks, and so on.

```
Scanner  →  Set of Tokens  →  Parser (Syntax Analyzer)  →  Syntax Structures
```

Simply put, the parser examines whether the source code written follows the **grammar** (production rules) of the language.

The syntax structure of programming languages — and even spoken languages — can be expressed using **BNF notation** (Backus–Naur Form). For example, in spoken English:

```
sentence    → noun-phrase verb-phrase
noun-phrase → article noun
article     → THE | A | AN | ...
noun        → STUDENT | BOOK | ...
verb-phrase → verb noun-phrase
verb        → READS | BUYS | ...
```

A derivation of a sentence from this grammar:

```
sentence
→ noun-phrase verb-phrase
→ article noun verb-phrase
→ THE noun verb-phrase
→ THE STUDENT verb-phrase
→ THE STUDENT verb noun-phrase
→ THE STUDENT READS noun-phrase
→ THE STUDENT READS article noun
→ THE STUDENT READS A noun
→ THE STUDENT READS A BOOK
```

In a similar manner, the parser tries to derive your source program from the **starting symbol** of the grammar.

> **Note — Syntax vs. Semantics:** A sentence like *THE BOOK BUYS A STUDENT* is syntactically correct but semantically meaningless. The parser only verifies syntactic correctness; semantic analysis is performed later by the semantic analyzer.

> **Note — BNF Notation:** The formal BNF notation uses angle brackets and `::=`, for example `⟨sentence⟩ ::= ⟨noun-phrase⟩⟨verb-phrase⟩`. We use the simpler arrow notation throughout this chapter since it is easier to work with.

---

## Connecting the Scanner and Parser

The first two compilation phases work together to understand the structure of source code:

```
Source Code (characters)
        ↓  [uses Regular Grammars]
    Scanner (Chapter 4)
        ↓  Tokens
    Parser (Chapter 5)
        ↓  [uses Context-Free Grammars]
  Syntax Tree (Program Structure)
```

**Example: From Characters to Syntax**

Consider the line of source code: `while (x >= 100)`

**Phase 1 – Scanner (Lexical Analysis):**

The scanner processes the character stream and identifies individual tokens using Regular Expressions / FSA:

- `while` → matches the pattern for keywords
- `x` → matches `L(L|d)*` for identifiers
- `>=` → matches the pattern for operators
- `100` → matches `[+|−]d+` for integers
- `(` and `)` → special symbols

Output of Scanner: `[while, (, x, >=, 100, )]`

**Phase 2 – Parser (Syntax Analysis):**

The parser takes the token stream and verifies it forms valid syntax structures according to Context-Free Grammar production rules such as:

```
while-stmt  → WHILE ( condition ) statement
condition   → expression op expression
expression  → identifier | number
```

Output of Parser: A **syntax tree** representing the program structure.

**Understanding "Language" in Both Contexts:**

| | Chapter 4: Scanner | Chapter 5: Parser |
|---|---|---|
| Language means | Pattern for ONE token type | Structure of the ENTIRE program |
| Example | `L(L|d)*` represents all valid identifiers | `L(G)` represents all valid programs |
| Uses | Regular Grammars | Context-Free Grammars |
| Power | Simpler, less powerful | More powerful, more complex |
| Task | Groups characters into tokens | Groups tokens into syntax structures |

[*Chapter 5, Part 2 – Grammar*](../ch5-part2-grammar/)
