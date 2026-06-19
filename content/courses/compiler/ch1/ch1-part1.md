---
title: "Chapter 1, Part 1 – Nature of the Course & What is a Programming Language"
date: 2026-06-19
weight: 1
toc: true
tags: ["programming-languages", "intro", "definitions"]
description: "Why we study PL concepts and paradigms, and the formal definition of a programming language."
---

# Chapter 1 - Introduction and Background
## Part 1: Nature of the Course & What is a Programming Language

---

## Nature of the Course

There are 4 ways to teach a programming languages course:

1. How to program in several languages
2. Survey of the history and nature of several languages
3. How to implement programming languages
4. The conceptual issues of programming languages (concepts and paradigms)

This course focuses on the 4th approach. We study the structural issues of programming languages, summed up in two words: **Concepts and Paradigms**.

- **Concepts**: the basic structure of a PL, like syntax, semantics, data types, control structures
- **Paradigms**: the model or approach used to reason about and solve a problem

---

## Three Views of a Programming Language

| View | Who |
|------|-----|
| Designer | The inventor of the language |
| Implementer | The one who builds the compiler or interpreter |
| User | The one who writes programs in the language |

The course covers all three, with a bit more focus on the implementer side.

---

## Why Study Programming Language Concepts

1. Increases your ability to express programming ideas
2. Understanding a language's structure makes it easier to learn other languages
3. Increases your ability to design new languages (example: the if/else statement is ambiguous, so a language designer has to account for that ambiguity when writing production rules)
4. Pushes computing forward as a field overall

---

## What is a Programming Language?

A language is a system of signs used to communicate. All languages, spoken or programmed, have grammar and vocabulary.

In programming languages, grammar comes from a set of **Production Rules**. The key difference from spoken language is that these rules are strict, with no exceptions.

General definition:

> A programming language is a system of signs used by a person to communicate with a computer.

More specific definition:

> A programming language is a notational system for describing **computation** in **machine readable** and **human readable** form.

### The Three Key Concepts

**1. Computation**

This is what computers actually do. Everything in a computer eventually breaks down into small computational steps, mostly arithmetic.

**2. Machine Readable**

There needs to be an algorithm that translates the code in an unambiguous, finite way. This algorithm should be simple and run in time proportional to the program's size. We get machine readability by restricting the syntax to a **Context-Free Grammar (CFG)**, which lets us build translator algorithms.

**3. Human Readable**

A program also has to be readable by people. This is where high-level languages come in, through **abstraction**:

- **Data Abstraction**: giving variables and types meaningful names
  - Simple: int, char
  - Structured: arrays, strings
- **Control Abstraction**: grouping instructions
  - Simple: an assignment like `X = X + 3` (fetch X, add 3, store back) packed into one statement
  - Structured: if/else, case, while, procedures, functions, blocks

---

## Two Parts of a Programming Language

For a more precise definition, a programming language splits into:

1. **Syntax** - the structure
2. **Semantics** - the meaning

This is the concrete definition we'll work with going forward.
