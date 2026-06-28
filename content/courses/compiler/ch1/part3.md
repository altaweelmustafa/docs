---
title: "Chapter 1, Part 3 – Paradigms of Programming Languages"
date: 2026-06-19
weight: 3
toc: true
tags: ["programming-languages", "paradigms", "lisp", "prolog", "oop"]
description: "Imperative, functional, logical, and object oriented paradigms with code examples."
author: "Mustafa Altaweel, Samaa Kali"
---

## Imperative / Procedural Paradigm

Also called the **Von Neumann model**, based on single processor sequential execution of instructions. Languages in this paradigm are characterized by:

1. Sequential execution of instructions
2. Using variables to represent memory locations
3. Using assignment statements to change a variable's value

Pascal and C are designed around this paradigm.

**Example: GCD in Pascal**

```pascal
function gcd(x,y:integer):integer;
Begin
  If (x = y) then
    gcd:=x
  else
    if (x > y) then
      gcd:=gcd(x-y,y)
    else
      gcd:=gcd(x,y-x);
End
```

**Same program in C**

```c
int gcd(int n, int m)
{
    if(n==m){
        return m;
    }
    else{
        if(n>m)
            return gcd(n-m,m);
        else
            return gcd(n,m-n);
    }
}
```

---

## Functional Paradigm

Computation is based on evaluating or calling functions. Sometimes called an "applicative" language. Characterized by:

1. No notion of variables or assignment statements
2. No loops, repetition happens through recursive calls

**LISP** (LISt Programming) is the classic example. In LISP, everything is a list:

> A list is a sequence of things separated by blanks and surrounded by parentheses.

Examples:

```lisp
(+ a b)
(+ 2 3)
(if a b c)   ; if a is true, value is b, otherwise value is c
```

**Small LISP programs**

```lisp
>(defun f(x)
   (+ x 1))
>f
>(f 3)
>4
```

```lisp
>(defun ff(x y)
   (+ x y))
>ff
>(ff 3 5)
>5
```

**GCD in LISP**

```lisp
>(defun gcd(n m)
   (if (= n m) n
       (if(> n m) (gcd (- n m) m)
           (gcd (n (- m n))))))
>gcd
>(gcd 18 16)
>2
```

**Power function (x^n) in LISP**

```lisp
>(defun pwr(x n)
   (if (= n 0) 1
       (* x (pwr x (- n 1)))))
>pwr
>(pwr 2 4)
>16
```

---

## Logical Paradigm

Based on symbolic logic. The program is a set of statements describing what's true. **PROLOG** (PROgramming LOGical) is the classic example.

**GCD in PROLOG**

```prolog
gcd(u,v,u) :- v = 0.
gcd(u,v,x) :- v > 0,
              y is u mod v,
              gcd(v,y,x).
```

---

## Object Oriented Paradigm

Introduces the notions of **Object** and **Class**. Became widespread in the 90s. Advantages include:

- Encapsulation of data and functions
- Inheritance
- Polymorphism

---

## Language Evolution

The general lineage runs: **Imperative -> Functional -> Logical -> Object Oriented**, with each paradigm branching off and influencing later language design.
