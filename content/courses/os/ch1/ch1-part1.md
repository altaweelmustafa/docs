---
title: "Chapter 1, Part 1 – OS Goals, Computer Structure & History"
date: 2026-06-28
weight: 1
toc: true
tags: ["operating-systems", "goals", "hardware", "history", "throughput"]
description: "Operating system goals, the four layers of a computer system, OS views, and the history of early batch systems."
author: "Mustafa Altaweel"
---

## Operating System Goals

An **Operating System (OS)** is a set of algorithms that run the computer machine. It manages the computer resources and must do so **efficiently**.

The OS has three goals:

1. **Overall goal** — Execute user programs.
2. **Primary goal** — Conveniency: it's easier for the user to interact with the OS than to deal directly with machine/assembly language.
3. **Secondary goal** — Efficiency: make the best use of available hardware resources.

### Other Goals: Utilization of Computer Resources

The OS must maximize the utilization of all computer resources:

- **CPU Utilization** — keep the CPU as busy as possible.
- **Memory Utilization** — use memory as much as possible.
- **I/O Device Utilization** — keep I/O devices active.

> **Throughput**: the number of jobs (programs) that finish execution per unit of time. System performance is measured with throughput.

---

## Computer Resources

The three main computer resources managed by the OS:

1. **CPU**
2. **Memory**
3. **I/O Devices**

---

## Computer System Structure

A computer system is composed of four layers:

| Layer                       | Description                                                                                                                                                                                          |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Hardware**             | Physical devices (chips, wires, power supplies) + Microprogram (primitive software that communicates with physical devices — an interpreter that fetches and executes machine language instructions) |
| **2. Operating System**     | Controls and coordinates use of hardware among users and applications                                                                                                                                |
| **3. Application Packages** | Compilers, databases, etc.                                                                                                                                                                           |
| **4. User Programs**        | Programs written and run by users                                                                                                                                                                    |

### Machine Language (Assembly Language)

The **microprogram** fetches and executes machine language instructions. It acts as an interpreter between the hardware and the OS.

---

## Operating System Views (OS Goals)

The OS can be viewed from multiple perspectives, each corresponding to a goal:

1. **Control Program** _(Overall goal)_ — controls the execution of all programs to prevent errors and improper use of the computer.

2. **Extended Machine** _(Primary goal)_ — an extension of the physical machine. It hides all the complexity of system programming and provides the user with a simple, clean machine to deal with. The user doesn't have to deal with machine/assembly language.

3. **Resource Manager** _(Secondary goal)_ — manages the computer resources (CPU, memory, I/O) efficiently.

4. **Kernel** — the part of the OS that is always running and executing instructions.

---

## History & Evolution of the OS

### Early Systems (First Generation)

- Programs were written on **punch cards** — each line of code required one card. A 200-line program required 200 cards.
- Input: card reader → computer → Output: printer/paper/tape.
- **Hexadecimal** was used for programming.

### Early Software Tools

Early software was developed to ease programming:

- **Machine Language** — direct binary instructions.
- **Assembly Language** (Assemblers) — symbolic representation of machine instructions.
- **Loaders** — load programs into memory.
- **Linkers** — link software additions (libraries) to programs.
- **Compilers** — translate high-level language to machine code.

### Why Performance Was Poor

- A great deal of time was wasted in **setup time**.
- **No overlap** between I/O and CPU execution.
- **Low CPU utilization** due to the big speed difference between I/O and CPU.

**Example:**

> A fast card reader can read 1200 cards/min = 20 cards/sec.
> The CPU can process 300 cards/sec.
> Each job: 60 sec card reading + 4 sec CPU.
> CPU utilization = 4 / 64 ≈ **6%**

The CPU sat idle for 94% of the time waiting for I/O.
