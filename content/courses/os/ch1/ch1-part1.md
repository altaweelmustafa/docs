---
title: "Chapter 1, Part 1 – What Operating Systems Do & Computer-System Organization"
date: 2026-06-28
weight: 1
toc: true
tags:
  [
    "operating-systems",
    "introduction",
    "computer-organization",
    "bootstrap",
    "interrupts",
  ]
description: "Defining the OS from the user and system perspective, and how CPUs, memory, and I/O devices cooperate through a shared bus."
author: "Mustafa Altaweel"
---

## What is an Operating System?

An **Operating System (OS)** is a program that acts as an intermediary between the user of a computer and the computer hardware. Its goals are:

- Execute user programs and make problem-solving easier.
- Make the computer system convenient to use.
- Use the computer hardware in an efficient manner.

A computer system can be divided into four components:

1. **Hardware** – CPU, memory, and I/O devices; the basic computing resources.
2. **Operating System** – controls and coordinates hardware use among applications and users.
3. **Application Programs** – word processors, compilers, browsers, games; define how resources are used to solve user problems.
4. **Users** – people, machines, or other computers.

### User View

From the user's perspective, the OS is designed mostly for **ease of use**, with some attention to performance and none to resource utilization. Embedded or dedicated devices (smart TVs, car systems) have little or no user interface at all.

### System View

From the system's perspective the OS plays two roles:

- **Resource Allocator** – manages all resources (CPU time, memory space, I/O devices) and decides between conflicting requests to achieve fair and efficient use.
- **Control Program** – controls the execution of user programs to prevent errors and improper use of the computer.

> **Kernel**: the one program running at all times on the computer. Everything else is either a system program or an application program.

---

## Computer-System Organization

### Basic Hardware Setup

One or more CPUs and device controllers connect through a common **bus**, providing access to shared memory. CPUs and device controllers can execute concurrently, competing for memory cycles.

Each device controller:

- Is in charge of a specific device type (disk, keyboard, video card).
- Has a local buffer.
- Moves data between its local buffer and main memory.

### Bootstrap Program

The **bootstrap program** is the very first program that runs when the computer is powered on or rebooted.

- Stored in **ROM** or **EPROM** (known as **firmware**).
- Initializes all aspects of the system (CPU registers, device controllers, memory contents).
- Loads the OS kernel into memory and starts its execution.

### Interrupts

The OS is largely **interrupt-driven**. Hardware can trigger an interrupt at any time by sending a signal to the CPU via the system bus. Software triggers an interrupt via a **system call** (also called a monitor call).

When an interrupt occurs:

1. The CPU stops what it is doing.
2. Control transfers to a fixed location containing the start address of the **interrupt service routine (ISR)**.
3. The ISR executes.
4. The CPU resumes the interrupted computation.

Interrupts are managed through an **interrupt vector** — a table of addresses for all interrupt service routines, usually stored in low memory.

- **Interrupt request line**: checked by the CPU after each instruction.
- **Maskable interrupts**: can be disabled temporarily.
- **Non-maskable interrupts**: reserved for events like unrecoverable memory errors.

### Storage Structure

The CPU can only load instructions from **main memory (RAM)**. Programs must be loaded into RAM to run.

- **Main memory**: volatile; loses data on power loss; implemented as **DRAM**.
- **Secondary storage**: non-volatile, large-capacity; magnetic disks, SSDs.
- **ROM**: stores firmware; non-volatile; cannot be written.
- **EEPROM**: can be changed but infrequently (e.g., smartphone firmware).

**Storage unit sizes:**

| Unit     | Size         |
| -------- | ------------ |
| Bit      | 0 or 1       |
| Byte     | 8 bits       |
| Kilobyte | 1,024 bytes  |
| Megabyte | 1,024² bytes |
| Gigabyte | 1,024³ bytes |
| Terabyte | 1,024⁴ bytes |
| Petabyte | 1,024⁵ bytes |

### I/O Structure

Each device controller maintains a local buffer and a set of special-purpose registers. Device drivers provide a uniform interface between the controller and the rest of the OS.

**I/O cycle (interrupt-driven):**

1. Driver loads registers in controller.
2. Controller examines registers and starts transfer to local buffer.
3. On completion, controller raises an interrupt.
4. Driver returns control (with data or status) to the OS.

**DMA (Direct Memory Access)**: for high-speed devices, the controller transfers an entire block of data directly to/from memory without CPU intervention. Only one interrupt is raised per block (not per byte).
