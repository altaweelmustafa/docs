---
title: "Ch2, Part 1 - VLANs and Trunking"
date: 2026-08-14
description: "VLAN basics, access vs trunk, 802.1Q tagging, native VLAN, key commands."
tags: [ccna, vlans, trunking, 802.1q, chapter2]
toc: true
weight: 1
---

## VLAN basics

- VLAN 1 is the default VLAN on every switch, cannot be deleted, carries CDP/DTP/VTP/STP traffic by default.
- Normal range: 1-1005. Extended range: 1006-4094.
- Each VLAN is its own broadcast domain, needs a router (or L3 switch/SVI) to talk to other VLANs.
- Voice VLAN: separate VLAN for IP phones so voice and data traffic on the same access port don't share a broadcast domain.

## Access vs trunk ports

| Port type | Carries | Command |
|---|---|---|
| Access | one VLAN, untagged | `switchport mode access` / `switchport access vlan X` |
| Trunk | multiple VLANs, tagged | `switchport mode trunk` / `switchport trunk allowed vlan X` |

## 802.1Q tagging

- Inserts a 4-byte tag into the Ethernet frame: 12 bits for VLAN ID (1-4094), plus a 3-bit priority field (CoS).
- Native VLAN traffic is sent untagged across a trunk (default VLAN 1).
- **Native VLAN mismatch** between two trunk ends is a classic exam trap: it doesn't stop the trunk, but causes VLAN leakage and CDP will flag it as a native VLAN mismatch warning.

## DTP (Dynamic Trunking Protocol)

Cisco proprietary, auto-negotiates access/trunk. Best practice (and often the "correct" exam answer for security): disable it with `switchport nonegotiate` and hardcode the mode.

| Mode | Negotiates to trunk with |
|---|---|
| dynamic auto | trunk, dynamic desirable, or trunk-set peer |
| dynamic desirable | trunk, dynamic auto, dynamic desirable |
| trunk | always trunk |
| access | always access |

## Key commands

```
vlan 10
 name SALES
interface fa0/1
 switchport mode access
 switchport access vlan 10
interface fa0/24
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
 switchport trunk native vlan 99
```
