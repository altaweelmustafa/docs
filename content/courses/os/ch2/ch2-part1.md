---
title: "Chapter 2 – Computer System Operations"
date: 2026-06-28
weight: 3
toc: true
tags:
  [
    "operating-systems",
    "interrupts",
    "DMA",
    "storage",
    "cache",
    "hardware-protection",
    "dual-mode",
  ]
description: "How the CPU, device controllers, and memory cooperate; interrupt handling; I/O types; DMA; storage hierarchy; cache; and hardware protection mechanisms."
author: "Mustafa Altaweel"
---

## Computer System Operations

### Hardware Setup

```
CPU ← → Disk Controller
         Tape Controller
         Printer Controller
         Device Controller(s)
              ↕
           Main Bus
              ↕
       Memory Controller → Memory
```

Key facts:

- **I/O can run concurrently with the CPU.**
- Each **device controller** is in charge of a particular device type.
- Each device controller has a **local buffer**.
- The CPU moves data **from/to main memory** to/from the local buffers.
- I/O transfers data from the device to the **local buffer of the controller**.
- The device controller informs the CPU that it has finished its operation by causing an **interrupt**.

---

## Interrupts

An **interrupt** is a signal sent to the CPU by:

- **Hardware** (e.g. I/O completion) — hardware interrupt.
- **Software** — called a **Trap** (e.g. division by zero, invalid memory access, request for OS service).

### Examples of Interrupts

| Cause                          | Type                      |
| ------------------------------ | ------------------------- |
| Completion of an I/O operation | Hardware Interrupt        |
| Division by zero               | Software Interrupt (Trap) |
| Invalid memory access          | Hardware Interrupt        |
| Request of an OS service       | System Call (Trap)        |

> **Operating Systems are interrupt-driven** — the CPU is "interrupted" to handle events.

### OS Services Can Be Requested Via

1. **System Program** — e.g. `format a:`, `copy A.dat B.dat`
2. **System Call** — an assembly language instruction.

### Interrupt Handling

There are two methods for handling interrupts:

#### (1) Interrupt Vector (Table)

A table stored in memory containing the **addresses** of all interrupt service routines.

**Example:**

- An interrupt arrives from the hard disk (number 45).
- CPU looks up entry [45] in the interrupt vector.
- Entry [45] holds the address `0x25164`.
- CPU jumps to the service routine at that address and executes it.

#### (2) By Polling

The CPU periodically checks each device to see if it needs service (less efficient).

---

## I/O Interrupt Structure

Each device has a **device controller** that includes a local buffer and an **Instruction Register (IR)**. When an I/O instruction is fetched from your program:

1. The instruction is loaded into the IR of the device controller.
2. The device controller fetches data from the hard disk into its local buffer.
3. When done, it raises an interrupt.

### Two Types of I/O

| Type                 | Behavior                                                                                                                                             |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Synchronous I/O**  | After I/O starts, control returns to the program **only upon I/O completion**. The CPU waits (busy-wait / `loop: jmp loop`). CPU is idle during I/O. |
| **Asynchronous I/O** | After I/O starts, control switches to **another program** without waiting for I/O completion. CPU stays productive.                                  |

---

## Direct Memory Access (DMA)

### Problem with slow devices (e.g. keyboard)

- Keyboard sends 1 character every **1 millisecond** (1 ms = 1000 µs).
- CPU needs **2 µs** for the service routine to handle each interrupt.
- CPU is left with 1000 − 2 = **998 µs** free → interrupt-driven I/O is fine here.

### Problem with fast devices (e.g. Hard Disk)

- Hard disk can send/receive a character every **4 µs**.
- CPU needs **2 µs** per interrupt → CPU is left with only 4 − 2 = **2 µs** free.
- The CPU is overwhelmed by constant interrupts → very inefficient.

### DMA Solution

- The OS sends **one entire block** of data at a time.
- Sends only **one interrupt** per block (not per byte).
- The device controller transfers the block **directly** between its local buffer and main memory, without CPU intervention.

```
Memory ← (one block of data) ← Local Buffer ← Device
                                     ↑
                               CPU only interrupted once
```

---

## Primary Storage (Main Memory — RAM) "Volatile"

- Memory is the largest storage medium accessed **directly** by the CPU.
- Memory is an **array of words**, each having an **address**.
- Word size = **2–8 bytes** (most common: 4 bytes).

### CPU Instructions on Memory

| Instruction | Description                                                                   |
| ----------- | ----------------------------------------------------------------------------- |
| **Load**    | Fetch (get) an instruction from memory into the **Instruction Register (IR)** |
| **Store**   | Store a register's value into a memory location                               |

### Instruction Cycle

The CPU repeatedly executes this cycle:

1. **Fetch** — get next instruction from memory into IR.
2. **Decode** — analyze the instruction.
3. **Execute** — perform the operation with the given operands.
4. **Store the result** — write the result back to memory.

---

## Secondary Storage

**Hard disks, tapes, CDs, Flash memories...**

Factors that affect secondary storage:

1. **Speed**
2. **Cost**
3. **Volatility** (مؤقت / دائم)

### Storage Hierarchy (fastest → slowest)

```
Registers
    ↓
Caches
    ↓
RAM (Main Memory)
    ↓
Hard Disk (HD)
    ↓
Tapes
    ↓ (slower)
  ...etc
```

---

## Cache Memory

**Caching** is copying data to a faster storage medium to speed up execution and ensure good performance.

### Examples of Caching

1. Memory (RAM) is considered a **cache for the Hard Disk**.
2. Registers are considered a **cache for Memory**.
3. **Instruction Cache Register (ICR)** — caches the next instruction before the CPU needs it.

### ICR Operation

- Steps ① and ② (fetching from memory into ICR, and ICR to IR) run **concurrently (in parallel)**.
- Fetching from ICR to IR is much faster than fetching from RAM to IR directly.

### Cache Levels

| Level    | Description                            |
| -------- | -------------------------------------- |
| **L1**   | Fastest — built within the CPU itself. |
| **L2**   | Bigger but slower than L1.             |
| L3, etc. | Even bigger and slower.                |

---

## Hardware Protection

The OS must protect computer resources. There are 3 areas to protect:

1. **I/O devices**
2. **Memory**
3. **CPU**

---

## Dual Mode of Operation

The OS runs in **two modes** to protect itself and resources:

| Mode                          | Mode Bit | Description                                                                       |
| ----------------------------- | -------- | --------------------------------------------------------------------------------- |
| **Monitor Mode** (Supervisor) | 0        | OS executes on behalf of itself (e.g. handling interrupts). Full hardware access. |
| **User Mode**                 | 1        | OS executes user programs. Restricted access.                                     |

### Implementation

One hardware bit called the **"mode bit"** indicates the current mode:

- `0` → Monitor mode
- `1` → User mode

> **Privileged Instruction**: an instruction that can only be executed in **monitor mode** (i.e. by the OS).

---

## I/O Protection

**All I/O instructions are made Privileged Instructions.**

This means only the OS (in monitor mode) can execute I/O instructions. User programs must request I/O via a system call.

---

## Memory Protection

To protect the memory allocation space of user programs and the OS itself, the hardware uses two registers:

- **Base Register** — holds the starting physical address of the process.
- **Limit Register** — holds the size of the memory region.

**Key concepts:**

- **Logical Address (LA)** — the offset of an instruction in your program; the address seen in your program.
- **Physical Address (PA)** — the actual address in memory.

```
PA = Logical Address + Base Register
```

**Example:** If Base Register = 4262, Limit Register = 1000, and LA = 738:

```
PA = 738 + 4262 = 4998
```

### How the OS Computes the Physical Address

```
CPU generates LA
      ↓
  LA < Limit Register?  → No → Memory Fault (trap)
      ↓ Yes
  PA = LA + Base Register
      ↓
  PA < (Base + Limit)?  → No → Memory Fault
      ↓ Yes
  Access granted → PA
```

---

## CPU Protection

A **timer** is used to prevent a user program from running forever and monopolizing the CPU.

**How it works:**

1. OS loads an integer value (e.g. 10,000) into the timer — this represents a number of **clock ticks**.
2. With every clock tick, the timer is decremented by 1.
3. When the timer reaches **zero**, it sends an **interrupt** to the CPU.
4. The CPU receives the interrupt and executes the **interrupt service routine**, which is responsible for **checking and reclaiming the CPU**.

> The timer can also be used for **computer time calculation**.

---

## Operating System Structure

The OS provides three main management functions:

1. **Process Management**
2. **Memory Management**
3. **File System (I/O) Management**

OS services can be provided by two methods:

- **System Call** — an assembly language instruction.
- **System Programs** — higher-level programs that use system calls.
