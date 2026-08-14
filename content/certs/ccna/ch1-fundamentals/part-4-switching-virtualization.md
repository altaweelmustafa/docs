---
title: "Ch1, Part 4 - Switching Concepts and Virtualization"
date: 2026-08-14
description: "MAC address table behavior, switching methods, collision/broadcast domains, VM vs container."
tags: [ccna, switching, virtualization, chapter1]
toc: true
weight: 4
---

## MAC address table (CAM table)

- Switch learns source MAC + port on every incoming frame.
- Unknown destination MAC: flood out all ports except the one it came in on.
- Known destination MAC: forward only to that port.
- Broadcast/multicast: always flooded (within the VLAN).
- Aging timer default: 300 seconds.

## Switching methods

| Method | Behavior | Latency | Error checking |
|---|---|---|---|
| Store-and-forward | Buffers entire frame, checks CRC | Highest | Yes, drops bad frames |
| Cut-through | Forwards after reading destination MAC | Lowest | No |
| Fragment-free | Reads first 64 bytes (past collision window) | Middle | Partial |

## Domains

- Collision domain: a segment where frames can collide. Every switch port is its own collision domain (full duplex effectively eliminates collisions).
- Broadcast domain: a segment where broadcasts propagate. Bounded by routers, or by VLAN boundaries on switches.

## Virtualization basics

| Concept | Notes |
|---|---|
| Type 1 hypervisor | Runs directly on hardware (ESXi, Hyper-V). Bare metal. |
| Type 2 hypervisor | Runs on top of a host OS (VirtualBox, VMware Workstation). |
| VM | Full guest OS, isolated via hypervisor. Heavier. |
| Container | Shares host OS kernel, isolated via namespaces. Lighter, faster startup. |

Exam framing: containers are more efficient/portable than VMs because they don't each carry a full OS.
