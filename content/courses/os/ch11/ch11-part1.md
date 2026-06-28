---
title: "Chapter 11 – File System: Access Methods & Disk Allocation"
date: 2026-05-29
weight: 1
toc: true
tags:
  ["os", "file-system", "disk-allocation", "contiguous", "linked", "indexed"]
description: "File access methods, the device directory, and the three main disk space allocation methods: contiguous, linked, and indexed."
---

## File Access Methods

Every file has:

- **Location:** Address of the first block in the file (constant, can't be changed).
- **File Pointer (FP):** The current position where the next read/write will occur.

### Sequential Access

Read/write the **next** block in sequence.

Example: If FP is at block 7, sequential access reads/writes block 8.

### Direct Access

Read/write **any block** by its relative number `n` within the file.

---

## Device Directory

Every storage device has a **device directory** — essentially a hash table containing information about all files stored on that device.

| Name    | Location | FP  | Size |
| ------- | -------- | --- | ---- |
| sam.txt | 11       | 11  | 6    |

The **Location** field stores the address of the file's first block.

---

## Disk Allocation Methods

How are disk blocks selected and allocated to a file?

### (1) Contiguous Allocation

The OS selects a run of **contiguous blocks** for the entire file.

**Device Directory entry:** `(Name, Location, Size)`

**Example:** `test.dat` starts at block 13, size 7 → occupies blocks 13–19.

**Accessing block #n:** `PA = Location + n − 1`

**Advantages:**

- Supports both sequential and direct access easily.
- Sequential: next block = FP + 1.
- Direct: `PA = Location + n − 1`.

**Disadvantages:**

1. **Fragmentation (holes)** → Solution: Compaction (defrag).
2. **Major problem:** What if the file grows? What if we need to insert or delete a block mid-file? Requires relocating the entire file.

---

### (2) Linked Allocation

File blocks are scattered on disk, each block contains a **pointer to the next block** — forming a linked list.

**Device Directory entry:** `(Name, Location, Size)` — Location = first block only.

**Advantages:**

- No fragmentation.
- File can grow, insert, and delete freely.

**Major Disadvantage:**

- Only supports **sequential access** easily.
- Direct access requires traversing the entire chain — slow.
- Solution: Use an **index file** that lists all block addresses in order.

---

### (3) Indexed Allocation

Each file has at least one **index block** that contains the addresses of all of the file's data blocks.

The device directory stores the address of the index block. The File Pointer (FP) points into the index.

**Accessing block #n:**

- Sequential: `next block = IND[n++]`
- Direct: `PA = IND[n]`

**Advantages:**

- Supports both sequential and direct access.

**Disadvantage:**

- Wasted space for the index block — every file needs at least one, even tiny files.
