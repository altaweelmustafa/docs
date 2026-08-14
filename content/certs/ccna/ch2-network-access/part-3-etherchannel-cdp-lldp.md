---
title: "Ch2, Part 3 - EtherChannel, CDP, LLDP"
date: 2026-08-14
description: "LACP vs PAgP, EtherChannel modes, CDP and LLDP comparison and defaults."
tags: [ccna, etherchannel, lacp, cdp, lldp, chapter2]
toc: true
weight: 3
---

## EtherChannel

Bundles multiple physical links into one logical link for redundancy and bandwidth. All ports in the bundle must match: speed, duplex, VLAN/trunk config.

| Protocol | Owner | Modes |
|---|---|---|
| LACP (802.3ad) | open standard | active / passive |
| PAgP | Cisco proprietary | desirable / auto |
| (none) | manual | on |

Negotiation pairing rules:

| Side A | Side B | Forms channel? |
|---|---|---|
| active | active or passive | Yes |
| passive | passive | No |
| desirable | desirable or auto | Yes |
| auto | auto | No |
| on | on | Yes (no negotiation) |
| on | anything else | No |

Layer 3 EtherChannel exists too (routed port bundling), same negotiation logic.

## CDP (Cisco Discovery Protocol)

- Cisco proprietary, Layer 2, discovers directly connected Cisco devices.
- Enabled by default on Cisco devices.
- Timer: 60 sec advertisement, 180 sec holdtime.
- `show cdp neighbors detail` gives IP address, platform, capabilities.

## LLDP (Link Layer Discovery Protocol)

- Open standard (802.1AB) equivalent to CDP, works across vendors.
- Not enabled by default on Cisco devices, must turn on with `lldp run`.
- Same general idea as CDP, timers differ slightly (default 30 sec advertisement).

Exam angle: if the topology has mixed vendors, LLDP is the answer, not CDP.
