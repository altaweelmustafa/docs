---
title: "Ch4, Part 4 - Hands-On Drills"
date: 2026-08-14
description: "Lab drills for DHCP, SSH remote access, NAT/PAT, and syslog/NTP."
tags: [ccna, drills, dhcp, nat, chapter4]
toc: true
weight: 4
---

## Easy

**1. DHCP pool with options.** 1 router, 1 switch, 2 PCs set to DHCP. Configure a DHCP pool with the correct network, default gateway, and a DNS server option, excluding the router's own IP from the pool. Verify: both PCs pull an IP automatically, and `show ip dhcp binding` lists both leases.

**2. SSH-only remote access.** 1 router. Set a domain name, generate RSA keys, create a local user, and configure the vty lines for SSH only (`transport input ssh`). Verify: you can SSH into the router from a PC, and a Telnet attempt is refused.

## Hard

**1. PAT plus DHCP relay together.** 3 routers: an "edge" router doing PAT to a simulated outside network, an "internal" router with a LAN subnet that has no local DHCP server, and a "DHCP server" router or a DHCP pool configured on the edge router. Configure `ip helper-address` on the internal router's LAN interface pointing at the DHCP server, and configure PAT (`overload`) on the edge router for outbound traffic. Verify: a PC on the internal LAN gets an IP via the relayed DHCP request, and its traffic to the "outside" network shows up correctly translated in `show ip nat translations`.

**2. NTP sync plus syslog on an event.** 2 routers: one as an NTP server (or use `ntp master`), one as a client syncing to it. Point the client's logging at a syslog server (a PC running a syslog service works in Packet Trace, or use `logging host`). Trigger a loggable event (interface flap, a port security violation, or an ACL deny with logging) and verify the syslog message arrives at the server with a timestamp that matches the NTP-synced clock, not the router's default clock.
