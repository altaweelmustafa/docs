---
title: "Ch3, Part 1 - Routing Table and Static Routing"
date: 2026-08-14
description: "Routing table components, administrative distance, longest prefix match, static route types."
tags: [ccna, routing, static-routes, administrative-distance, chapter3]
toc: true
weight: 1
---

## Routing table components

Each entry has: source code (C/S/O/D...), destination network + prefix, administrative distance, metric, next hop, outgoing interface, and how long it's been in the table.

`show ip route` codes: `C` connected, `L` local, `S` static, `O` OSPF, `D` EIGRP, `R` RIP, `*` candidate default.

## Administrative distance (memorize this table)

| Source | AD |
|---|---:|
| Connected | 0 |
| Static route | 1 |
| EIGRP (internal) | 90 |
| OSPF | 110 |
| IS-IS | 115 |
| RIP | 120 |
| EIGRP (external) | 170 |
| Unknown/unreachable | 255 |

Lower AD wins when multiple sources advertise the same route. This is trust between protocols, not route quality.

## Route selection logic

1. Longest prefix match wins first, always. A /26 is preferred over a /24 for the same destination, regardless of AD or protocol.
2. If prefix length ties, lowest AD wins.
3. If AD ties (same protocol), lowest metric wins.

## Static route types

| Type | Example |
|---|---|
| Standard static | `ip route 10.1.1.0 255.255.255.0 192.168.1.1` |
| Default static | `ip route 0.0.0.0 0.0.0.0 192.168.1.1` |
| Floating static | same as above but with a higher AD, used as backup: `ip route 10.1.1.0 255.255.255.0 192.168.1.1 200` |

Next hop can be specified as an IP address, an exit interface, or both. Using only an exit interface on a multi-access network can cause proxy ARP issues, using the next-hop IP is the safer/preferred form on the exam.

IPv6 static route: `ipv6 route 2001:db8::/64 2001:db8:1::1`
