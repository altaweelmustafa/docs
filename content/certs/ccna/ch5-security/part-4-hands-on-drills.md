---
title: "Ch5, Part 4 - Hands-On Drills"
date: 2026-08-14
description: "Lab drills for ACLs, port security, and DHCP snooping/DAI."
tags: [ccna, drills, acl, port-security, dai, chapter5]
toc: true
weight: 4
---

## Easy

**1. Standard ACL to block one host.** 1 router, 2 PCs, 1 server. Write a standard ACL that blocks only one specific PC from reaching the server while everything else can. Apply it in the correct direction, as close to the destination as the rules require. Verify: the blocked PC's pings fail, the other PC's pings succeed.

**2. Port security with sticky MAC.** 1 switch, 1 PC on an access port. Configure port security with a maximum of 1 MAC, sticky learning, and violation mode shutdown. Verify: the port learns the PC's MAC automatically, then connect a second device to the same port (or spoof a different MAC) and confirm the port goes err-disabled. Recover it with `shutdown` / `no shutdown`.

## Hard

**1. Extended ACL with a carve-out.** 1 router, a subnet of PCs, one management PC in that subnet, and a server. Write an extended ACL that allows only HTTP/HTTPS from the general subnet to the server, denies everything else from that subnet to the server, but still gives the management PC full access to everything. Verify each rule individually: general PC can reach the server on 80/443 but nothing else, management PC can reach anything, and confirm the rule order in `show access-lists` actually produces that behavior (order matters here).

**2. DHCP snooping plus DAI end to end.** 1 switch, 2 PCs on DHCP, 1 router as the DHCP server on a trusted uplink. Enable DHCP snooping on the VLAN, mark the uplink to the router as trusted. Enable DAI on the same VLAN. Let both PCs get real DHCP leases (building the snooping binding table). Then on one PC, manually set a static ARP entry (or static IP) that conflicts with the other PC's DHCP-assigned IP/MAC pairing, and confirm DAI drops the spoofed ARP while the legitimately-leased PC keeps working normally.
