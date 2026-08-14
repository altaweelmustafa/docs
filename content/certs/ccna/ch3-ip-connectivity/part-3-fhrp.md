---
title: "Ch3, Part 3 - First Hop Redundancy Protocols"
date: 2026-08-14
description: "HSRP, VRRP, GLBP compared: active/standby model, timers, virtual MAC."
tags: [ccna, fhrp, hsrp, vrrp, glbp, chapter3]
toc: true
weight: 3
---

## Why FHRP exists

Gives hosts a single default gateway IP that stays reachable even if the physical router providing it fails. Two or more routers share a virtual IP/MAC; one is active, the failover is transparent to hosts.

## Comparison

| Protocol | Owner | Roles | Load balancing |
|---|---|---|---|
| HSRP | Cisco proprietary | Active / Standby | No (per group, need multiple groups) |
| VRRP | Open standard | Master / Backup | No (same idea, standard-based) |
| GLBP | Cisco proprietary | AVG (Active Virtual Gateway) / AVF (Active Virtual Forwarder) | Yes, built in |

## HSRP details (the one the exam favors)

- Virtual MAC: `0000.0c07.acXX` where XX is the HSRP group number in hex.
- Default hello timer: 3 sec. Default hold timer: 10 sec.
- Default priority: 100. Higher priority wins active role.
- Preemption is off by default, if enabled, a router with higher priority reclaims active role when it comes back up.
- States: Initial, Learn, Listen, Speak, Standby, Active.

## GLBP details

Achieves load balancing by having the AVG hand out different virtual MACs (up to 4 AVFs) to different clients in round-robin, so multiple routers actively forward traffic instead of one sitting idle.

## Quick exam distinction

If the question says "Cisco only, simplest, one active router" think HSRP. If it says "multi-vendor / open standard" think VRRP. If it says "load balancing across gateways" think GLBP.
