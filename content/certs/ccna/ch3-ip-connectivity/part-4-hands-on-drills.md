---
title: "Ch3, Part 4 - Hands-On Drills"
date: 2026-08-14
description: "Lab drills for static routing, single-area OSPF, and FHRP."
tags: [ccna, drills, ospf, hsrp, chapter3]
toc: true
weight: 4
---

## Easy

**1. Static routes with a floating backup.** 2 routers with 2 paths between them (a primary link and a backup link). Configure static routes for full connectivity over the primary path. Add a floating static route over the backup path with a higher AD. Verify: connectivity works, then shut the primary link and confirm traffic fails over to the backup (`show ip route`, note the floating route appears only after the primary disappears).

**2. Basic single-area OSPF.** 2 routers on a point-to-point link. Enable OSPF area 0 on both. Verify: `show ip ospf neighbor` shows Full state, and the far LAN subnet appears in the routing table with an `O` code.

## Hard

**1. DR/BDR election and manipulation.** 3 routers on a shared multi-access segment (one switch, all three routers connected to it), single-area OSPF. Verify the elected DR and BDR with `show ip ospf neighbor`. Then change the OSPF priority on a non-DR router to force a new election (requires bouncing the interface or the OSPF process). Verify: the DR changes to match your priority change, and you can explain why the third router never reaches Full with the non-DR/BDR routers.

**2. HSRP gateway failover.** 2 routers sharing a LAN segment, PCs pointed at a single HSRP virtual IP as their gateway. Configure HSRP with explicit priorities and `preempt` on the intended primary. Verify active/standby roles with `show standby brief`. Shut the active router's LAN interface, time how long pings from a PC to an outside network pause, then bring the interface back up and confirm preemption reclaims the active role.

**3. AD vs longest-prefix-match conflict.** 2 routers, OSPF running between them plus a manually configured static route for the exact same destination network as an OSPF-learned route, both pointing at valid but different next hops. Verify: `show ip route` shows only one route installed, identify whether it's the static or the OSPF one and confirm it matches the AD table (static beats OSPF at the same prefix length). Then change the static route to a slightly more specific prefix (longer mask) for the same range and confirm it gets installed instead, regardless of AD, because of longest prefix match.
