---
title: "Ch2, Part 5 - Hands-On Drills"
date: 2026-08-14
description: "Lab drills for VLANs, trunking, STP, EtherChannel, and CDP/LLDP."
tags: [ccna, drills, vlans, stp, chapter2]
toc: true
weight: 5
---

## Easy

**1. Two VLANs, one switch.** 1 switch, 4 PCs. Create VLAN 10 and VLAN 20, assign 2 ports to each. Verify: PCs in the same VLAN ping each other, PCs in different VLANs cannot (no router yet).

**2. CDP and LLDP discovery.** 2 switches + 1 router, all connected. Use `show cdp neighbors detail` to find each neighbor's IP and platform. Then run `lldp run` on all devices and repeat with `show lldp neighbors detail`. Verify: you can list every neighbor's IP and device type from both protocols.

**3. EtherChannel with LACP.** 2 switches, 2 physical links between them. Bundle both links into a single LACP EtherChannel (active/active). Verify: `show etherchannel summary` shows the bundle up, and STP shows only the port-channel interface, not the individual members, in the topology.

## Hard

**1. Multi-switch VLANs + trunk + STP reconvergence.** 3 switches in a triangle, 2 VLANs spanning all three via trunks. Verify the elected root bridge and find the blocked port with `show spanning-tree`. Then lower the priority on a non-root switch to force a new root bridge election. Verify: the blocked port moves to a different switch, and you can explain why using the bridge ID.

**2. Native VLAN mismatch + EtherChannel mode mismatch fault-find.** Build a topology with a trunk between two switches with mismatched native VLANs, and a separate EtherChannel elsewhere with mismatched negotiation modes (e.g. LACP active on one side, PAgP desirable on the other, so it silently fails to bundle). Find and fix both using log messages, `show interfaces trunk`, and `show etherchannel summary`, without being told what's wrong going in.
