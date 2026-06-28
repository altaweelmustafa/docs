---
title: "Chapter 1, Part 4 – Storage Management, Protection & Security, Kernel Data Structures"
date: 2026-06-28
weight: 4
toc: true
tags: ["operating-systems", "storage", "file-system", "caching", "protection", "security", "data-structures"]
description: "The storage hierarchy, file-system management, caching, protection vs. security, and the core data structures the kernel uses."
author: "Mustafa Altaweel"
---

## Storage Management

The OS abstracts the physical properties of storage devices into a logical storage unit — the **file**. Each storage medium (disk, USB, tape) is controlled by a device driver that hides hardware details.

### File-System Management

Files are usually organized into **directories**. The OS manages:

- Creating and deleting files and directories.
- Primitives to manipulate files and directories.
- Mapping files onto secondary storage.
- Backing up files to non-volatile media.

Access control on most systems determines who can access what (read, write, execute).

### Mass-Storage Management

Since main memory is volatile and too small, the computer must provide **secondary storage** (usually magnetic disks or SSDs) to hold data and programs permanently.

OS responsibilities:

- Free-space management.
- Storage allocation.
- Disk scheduling (ordering read/write requests for efficiency).

**Tertiary storage** (optical disks, magnetic tape) is used for backups and archiving.

### Storage Hierarchy

Storage systems are organized in a hierarchy based on **speed**, **cost**, and **volatility**:

| Level | Type | Speed | Cost | Volatile? |
|---|---|---|---|---|
| 1 | Registers | Fastest | Highest | Yes |
| 2 | Cache | Very fast | High | Yes |
| 3 | Main memory (RAM) | Fast | Medium | Yes |
| 4 | SSD / NVM | Medium | Low | No |
| 5 | Magnetic disk (HDD) | Slow | Very low | No |
| 6 | Optical disk | Very slow | Lowest | No |
| 7 | Magnetic tape | Slowest | Lowest | No |

As we move up the hierarchy: faster, smaller, and more expensive. As we move down: slower, larger, and cheaper.

### Caching

**Caching** is a fundamental OS/hardware principle. Information is temporarily copied from a slower level to a faster one. The faster storage (cache) is checked first; if the data is there (**cache hit**), it is used directly. Otherwise (**cache miss**), data is copied from the slower storage into the cache.

Cache sizes are limited — **cache management** (deciding what to keep and what to evict) is an important design problem.

**Data migration example** — value `A` may reside at multiple levels simultaneously:

```
Magnetic disk → Main memory → Cache → Hardware register
```

In a multiprocessor environment, **cache coherency** must be maintained — all CPUs must see the most recent value of a shared datum.

---

## Protection and Security

### Protection

**Protection** is any mechanism for controlling the access of processes or users to resources defined by the computer system.

- Distinguishes between authorized and unauthorized use.
- Provides means to specify the controls to be imposed and enforce them.
- Prevents a malfunctioning process from interfering with others.

### Security

**Security** defends the system against internal and external attacks:

- Denial-of-service (DoS).
- Worms and viruses.
- Identity theft.
- Theft of service.

Systems generally distinguish between **user identities**:

- **User ID (UID)**: a unique number associated with each user; included with all files, processes, and threads.
- **Group ID (GID)**: allows sets of users to be defined and controlled as a group.
- **Privilege escalation**: allows a user to gain extra permissions temporarily (e.g., `sudo` in Linux).

---

## Kernel Data Structures

The OS kernel relies on classic data structures to manage resources efficiently. Understanding them is essential to understanding OS implementation.

### Lists

- **Array**: fixed-size, direct access by index.
- **Linked list**: variable size, efficient insertion/deletion; used heavily in the kernel.
  - Singly linked, doubly linked, circularly linked.

### Stacks and Queues

- **Stack**: LIFO — used for function call frames, interrupt handling.
- **Queue**: FIFO — used for CPU scheduling ready queues, I/O request queues.

### Trees

- **Binary search tree** (BST): O(log n) search in best case, O(n) worst case.
- **Balanced BST** (e.g., red-black tree): guaranteed O(log n) — Linux uses a red-black tree for CPU scheduling (`CFS`).

### Hash Maps

- Hash function maps a key to a bucket index.
- O(1) average lookup — used for file-system directory lookups, process tables, page tables.

### Bitmaps

A **bitmap** is a string of `n` binary digits representing the status of `n` items. Example: disk block availability (1 = free, 0 = used). Compact and fast for set operations.
