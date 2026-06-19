---
title: "Chapter 4, Part 1 – What is a Lexical Analyzer?"
date: 2026-05-29
weight: 1
toc: true
tags: ["compiler", "lexical-analysis", "scanner", "tokens"]
description: "How the scanner groups characters into tokens and builds the symbol table."
---

A **Lexical Analyzer** or **Scanner** is an algorithm that groups the characters of the source code to form **Tokens**, and returns the **Internal Representation Number** of these tokens — a kind of ID assigned to each token.

These tokens are divided into 3 kinds:

1. **Names** — any name in a program, divided into 3 types:
   - **Keywords/Reserved**: words such as `if`, `else`, `while`. These cannot be used as variable names.
   - **User Defined Names**: names declared by the user.
   - **Standard Identifiers**: such as `sin`, `cos`, `sqrt`. Like reserved words, these cannot be used as variable names and are not included in production rules.

2. **Values** — such as integers (`1, 2, 3`) or floating point (`1.1, 2.34`) etc.

3. **Special Symbols/Tokens** — logical (`==`, `&&`, `||`) and arithmetic operations (`+`, `-`, `*`, `/`), parentheses (`[]`, `{}`, `()`), or any other tokens not from the first or second kind.

**Example:** Applying the scanner to this code:

```
while(x>=100)
{
    n +=x;
    x++
}
```

Produces the tokens:

`while` , `(` , `x` , `>=` , `100` , `)` , `{` , `n` , `+=` , `x` , `;` , `x` , `++` , `}`

Referencing these against a keywords table gives each token its Internal Representation Number. Note that all user-defined names share the same number — to the syntax analyzer, it only matters that a variable exists, not what it's called.

---

## Type Checking Implementation

During analysis, the compiler builds the **Symbol Table** — constructed at declaration time. It contains user-defined names (mostly variables), their type, and their values.

**Example code:**

```c
int compute(int, int); // assume it does addition
int n;
float x = 3.5, y;
const int m = 10;
n += m;
y = x * 2;
n = compute(m, m) + 10;
```

**Initial Symbol Table:**

| Name    | Type          | Data type | Value |
| ------- | ------------- | --------- | ----- |
| compute | function-name | integer   | 0     |
| n       | variable      | integer   | 0     |
| x       | variable      | float     | 3.5   |
| y       | variable      | float     | 0     |
| m       | constant      | integer   | 10    |

**Final Symbol Table (after execution):**

| Name    | Type          | Data type | Value |
| ------- | ------------- | --------- | ----- |
| compute | function-name | integer   | 20    |
| n       | variable      | integer   | 30    |
| x       | variable      | float     | 3.5   |
| y       | variable      | float     | 7     |
| m       | constant      | integer   | 10    |

To perform type checking, the compiler takes the name and checks the keywords table. If not found there, it checks the symbol table. If the variable is not in the symbol table, it throws an **unknown symbol error**. If found but the operation is incompatible with the variable's type, it throws a **type mismatch error**.

At this stage the compiler maintains two main tables:

1. **Keywords Table** — contains all reserved words with their internal representation numbers.
2. **Symbol Table** — stores user-defined identifiers along with their type, data type, and values.

---

## Lexical Analysis Errors

When the scanner encounters a new word, it first compares it against the reserved words table. If not found, it treats it as a user-defined identifier and checks the symbol table. Errors that may occur:

1. **Illegal Character Error** — the scanner encounters a character not part of the language's alphabet (e.g. `@`, `#`, `$` where not defined). A valid token cannot be formed.

2. **Undeclared Variable Error** — a word is encountered that is not present in the symbol table. Error message: `"Undeclared variable"` or `"Unknown symbol"`.

3. **Type Mismatch Error** — the type retrieved from the symbol table is incompatible with the operation being performed (e.g. assigning a float to an integer variable).
