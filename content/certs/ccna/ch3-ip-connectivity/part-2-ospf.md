---
title: "Ch3, Part 2 - Single-Area OSPFv2"
date: 2026-08-14
description: "Neighbor states, DR/BDR election, router ID, cost, hello/dead timers, config syntax."
tags: [ccna, ospf, chapter3]
toc: true
weight: 2
---

## Core idea

Link-state protocol, builds a full topology map (LSDB) per area, runs Dijkstra SPF to compute best paths. CCNA only requires single-area (Area 0) OSPFv2.

## Neighbor states (memorize the order)

Down &rarr; Init &rarr; 2-Way &rarr; ExStart &rarr; Exchange &rarr; Loading &rarr; Full

- **2-Way**: neighbor relationship confirmed, DR/BDR election happens here on multi-access networks.
- **Full**: LSDBs fully synchronized, this is the healthy end state.
- On a broadcast segment, non-DR/BDR routers stay in **2-Way** with each other but reach **Full** with the DR and BDR only.

## Router ID selection (in order of priority)

1. Manually configured (`router-id X.X.X.X`).
2. Highest IP on any loopback interface.
3. Highest IP on any active physical interface.

## DR/BDR election

- Happens on multi-access segments (Ethernet), not on point-to-point links.
- Highest OSPF priority wins (default priority = 1). Priority 0 means the router can never become DR/BDR.
- Tie-break: highest router ID.
- Purpose: reduces adjacencies from full mesh to a hub-and-spoke via the DR, cuts down flooding traffic.

## Timers

| Network type | Hello | Dead |
|---|---:|---:|
| Broadcast (Ethernet) | 10 sec | 40 sec |
| Non-broadcast (NBMA) | 30 sec | 120 sec |

Dead interval is always 4x the hello interval. Timers must match between neighbors or the adjacency won't form.

## Cost

`Cost = reference bandwidth / interface bandwidth`. Default reference bandwidth = 100 Mbps. So a 100 Mbps interface = cost 1, a 10 Mbps interface = cost 10, anything faster than 100 Mbps (like Gigabit) also rounds to cost 1 unless you raise the reference bandwidth with `auto-cost reference-bandwidth`.

## Config essentials

```
router ospf 1
 router-id 1.1.1.1
 network 10.0.0.0 0.0.0.255 area 0
```

The `network` command uses a **wildcard mask**, not a subnet mask. Area must match on both sides of a link, area 0 is mandatory as the backbone if you have more than one area (not required for CCNA single-area scope, but know the term).

## Verification commands

`show ip ospf neighbor`, `show ip protocols`, `show ip ospf interface brief`.
