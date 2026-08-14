---
title: "Ch1, Part 5 - Hands-On Drills"
date: 2026-08-14
description: "Lab drills for IPv4/IPv6 addressing, subnetting, cabling, and switching basics."
tags: [ccna, drills, subnetting, chapter1]
toc: true
weight: 5
---

Use Packet Tracer (or real gear) for all of these. Don't just read the steps, build the topology.

## Easy

**1. Subnet a /24 into 4 equal subnets.** 1 router, 2 switches, 4 PCs (2 per switch). Split 192.168.1.0/24 into four /26 subnets, use two of them, configure router interfaces as gateways, assign static PC IPs. Verify: same-subnet PCs ping directly, cross-subnet pings work once router interfaces are up (`show ip interface brief`).

**2. Duplex/speed mismatch symptoms.** 2 switches connected by one link. Hardcode one side full-duplex/100Mbps and leave the other on auto. Generate traffic between PCs across the link. Verify: find late collisions or CRC errors in `show interfaces` on the mismatched port, then fix by matching both sides.

**3. IPv6 SLAAC vs manual EUI-64.** 1 router, 1 switch, 2 PCs. Enable IPv6 on the router LAN interface, turn on router advertisements. Let one PC get an address via SLAAC. On the other PC, manually calculate and assign the EUI-64 address it should have gotten. Verify: the SLAAC-assigned address matches your manual calculation.

## Hard

**1. VLSM design for uneven host counts.** 3 routers in a line (R1-R2-R3). LAN segments need 50, 20, and 10 hosts. Two point-to-point links between routers. From a single 192.168.0.0/24: write the full VLSM plan on paper first (largest requirement first, down to /30 or /31 links), then configure every interface to match. Verify: every interface IP matches your plan exactly, no subnet is bigger than it needs to be (`show ip interface brief` on all three routers).

**2. Blind duplex/cabling fault-find.** Build a 3-switch chain with several PCs. Have someone else (or yourself, a day later) inject one fault: a duplex mismatch, a bad "cable" (simulate via mismatched speed), or a wrong VLAN assumption. Using only `show interfaces`, `show interfaces status`, and counters, locate the fault without being told which link it's on. Verify: you can name the exact interface and the exact counter that gave it away.
