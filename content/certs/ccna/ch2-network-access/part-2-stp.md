---
title: "Ch2, Part 2 - Spanning Tree Protocol"
date: 2026-08-14
description: "Port roles, port states, timers, root bridge election, PortFast, BPDU Guard."
tags: [ccna, stp, rstp, chapter2]
toc: true
weight: 2
---

## Why STP exists

Prevents Layer 2 loops (broadcast storms, MAC table instability, duplicate frames) by blocking redundant paths and keeping only one active path to the root bridge.

## Root bridge election

- Lowest Bridge ID wins. Bridge ID = Priority (default 32768) + MAC address.
- Lower priority = more preferred. Set manually with `spanning-tree vlan X root primary` or `spanning-tree vlan X priority X`.

## Port roles

| Role | Meaning |
|---|---|
| Root port | best path to root bridge, one per non-root switch |
| Designated port | best path for a segment, one per segment |
| Non-designated (blocking/alternate) | not forwarding, prevents loop |

## Port states

| STP (802.1D) | RSTP (802.1w) | Forwards data? | Learns MAC? |
|---|---|---|---|
| Blocking | Discarding | No | No |
| Listening | Discarding | No | No |
| Learning | Learning | No | Yes |
| Forwarding | Forwarding | Yes | Yes |
| Disabled | Discarding | No | No |

## Timers (classic 802.1D)

| Timer | Default |
|---|---|
| Hello | 2 sec |
| Forward delay | 15 sec (listening + learning) |
| Max age | 20 sec |

Convergence time classic STP: ~30-50 sec. RSTP converges in seconds using proposal/agreement instead of waiting on timers, this is the main exam-testable advantage of RSTP.

**Rapid PVST+** is Cisco's default, one spanning tree instance per VLAN, RSTP-based.

## PortFast and BPDU Guard

- PortFast: skips listening/learning on access ports connected to end hosts, straight to forwarding. Never enable on a port that could connect to another switch.
- BPDU Guard: err-disables a PortFast port if it receives a BPDU (protects against an accidental switch/loop being plugged in). Pair these two together in practice and on the exam.
