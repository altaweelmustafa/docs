---
title: "Ch4, Part 3 - QoS Basics and Remote Access"
date: 2026-08-14
description: "QoS per-hop behaviors, DSCP vs CoS, SSH vs Telnet, TFTP vs FTP."
tags: [ccna, qos, ssh, tftp, chapter4]
toc: true
weight: 3
---

## QoS per-hop behaviors

| Step | What it does |
|---|---|
| Classification | Identifies traffic type (ACL, DSCP, protocol) |
| Marking | Tags the packet/frame (DSCP, CoS) so downstream devices trust it |
| Congestion management | Queuing strategies (e.g. priority queuing) when link is busy |
| Congestion avoidance | Drops packets proactively before the queue fully fills (e.g. WRED) |
| Policing | Drops traffic that exceeds a rate (no buffering) |
| Shaping | Buffers/delays traffic that exceeds a rate instead of dropping |

- DSCP: Layer 3 marking, in the IP header, 6 bits, survives end to end.
- CoS: Layer 2 marking, in the 802.1Q tag, only survives within the L2 domain (stripped at L3 boundary).
- Mark as close to the source as possible, at the trust boundary (edge switch or endpoint).

## Remote access

| Protocol | Port | Encrypted |
|---|---:|---|
| Telnet | 23 | No |
| SSH | 22 | Yes |

Always the "correct" exam answer for remote CLI management is SSH, Telnet is legacy/insecure.

## File transfer

| Protocol | Port | Transport |
|---|---:|---|
| FTP | 20 (data), 21 (control) | TCP |
| TFTP | 69 | UDP |

TFTP is simple, no authentication, commonly used for IOS image/config transfers to/from a router or switch.
