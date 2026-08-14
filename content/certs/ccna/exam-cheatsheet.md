---
title: "CCNA Exam Cheatsheet"
date: 2026-08-14
description: "The night-before page: every number, port, and comparison table in one place, no explanations."
tags: [ccna, cisco, exam, cheatsheet]
toc: true
weight: 99
---

## Port numbers

| Protocol | Port | Transport |
|---|---:|---|
| FTP | 20/21 | TCP |
| SSH | 22 | TCP |
| Telnet | 23 | TCP |
| DNS | 53 | TCP/UDP |
| DHCP | 67/68 | UDP |
| TFTP | 69 | UDP |
| HTTP | 80 | TCP |
| NTP | 123 | UDP |
| SNMP | 161/162 | UDP |
| Syslog | 514 | UDP |
| HTTPS | 443 | TCP |
| TACACS+ | 49 | TCP |
| RADIUS | 1812/1813 | UDP |

## Administrative distance

| Source | AD |
|---|---:|
| Connected | 0 |
| Static | 1 |
| EIGRP internal | 90 |
| OSPF | 110 |
| IS-IS | 115 |
| RIP | 120 |
| EIGRP external | 170 |
| Unreachable | 255 |

## Subnetting quick chart

| CIDR | Mask | Block | Usable hosts |
|---|---|---:|---:|
| /25 | .128 | 128 | 126 |
| /26 | .192 | 64 | 62 |
| /27 | .224 | 32 | 30 |
| /28 | .240 | 16 | 14 |
| /29 | .248 | 8 | 6 |
| /30 | .252 | 4 | 2 |

## STP timers and states

- Hello 2s, forward delay 15s, max age 20s (classic 802.1D).
- States: Blocking &rarr; Listening &rarr; Learning &rarr; Forwarding.
- Rapid PVST+ uses Discarding/Learning/Forwarding and converges in seconds.

## OSPF

- Neighbor states: Down &rarr; Init &rarr; 2-Way &rarr; ExStart &rarr; Exchange &rarr; Loading &rarr; Full.
- Hello 10s / Dead 40s on broadcast networks.
- Cost = 100 Mbps / interface bandwidth.
- Router ID: manual > highest loopback > highest active interface.

## FHRP

| Protocol | Owner | Roles | Load balances? |
|---|---|---|---|
| HSRP | Cisco | Active/Standby | No |
| VRRP | Standard | Master/Backup | No |
| GLBP | Cisco | AVG/AVF | Yes |

## AAA

| Protocol | Transport | Encrypts | Combines authn+authz? |
|---|---|---|---|
| TACACS+ | TCP 49 | full packet | No, separates all three |
| RADIUS | UDP 1812/1813 | password only | Yes |

## ACLs

- Standard: 1-99, 1300-1999, source only, place near destination.
- Extended: 100-199, 2000-2699, source+dest+port, place near source.
- Implicit deny at the end, always. Top-down, first match wins.
- Wildcard = inverse of subnet mask, 0 = match, 1 = don't care.

## NAT

- Static: 1:1. Dynamic: pool, many-to-many. PAT: many-to-1, `overload` keyword.
- Inside local = private, inside global = translated public.

## Syslog severity (0 worst, 7 best)

Emergency, Alert, Critical, Error, Warning, Notice, Informational, Debug.

## Wireless security

WEP (broken) &rarr; WPA (TKIP) &rarr; WPA2 (AES-CCMP) &rarr; WPA3 (AES-GCMP, SAE). PSK = shared passphrase, Enterprise = 802.1X/RADIUS per-user.

## Frequent exam traps

- Wildcard mask in ACLs/OSPF `network` statements, not a subnet mask.
- Native VLAN mismatch does not break the trunk, it leaks traffic and triggers a CDP warning.
- `/31` is valid for point-to-point links (RFC 3021), only 2 addresses, both usable.
- DAI requires DHCP snooping to already be enabled.
- LLDP is the vendor-neutral CDP equivalent, not enabled by default.
- Longest prefix match always wins before administrative distance is even considered.
- PortFast + BPDU Guard go together, PortFast alone is a loop risk on the wrong port.
- IPv6 has no broadcast, multicast/anycast cover that role.
- Ansible is agentless (SSH, push), Puppet/Chef are agent-based (pull).

## Last-night checklist

- Subnet a /24 into any prefix length in under 10 seconds, no paper.
- Recite the AD table and OSPF neighbor states from memory.
- Draw the DORA process and NAT term table (inside/outside local/global).
- Compare TACACS+ vs RADIUS and HSRP vs VRRP vs GLBP without looking.
- Write a standard and an extended ACL from a plain-English requirement.
- Explain port security violation modes and DHCP snooping trust.
- State the northbound/southbound API distinction and the Ansible vs Puppet difference.
