---
title: "Ch1, Part 3 - IPv6 Addressing"
date: 2026-08-14
description: "IPv6 address types, compression rules, EUI-64, SLAAC vs DHCPv6."
tags: [ccna, ipv6, chapter1]
toc: true
weight: 3
---

## Address types

| Type | Range/prefix | Notes |
|---|---|---|
| Global unicast | 2000::/3 | routable on the internet, like public IPv4 |
| Link-local | FE80::/10 | auto-assigned, required on every interface, not routed |
| Unique local | FC00::/7 (FD00::/8 in practice) | like RFC 1918, private, routable inside org |
| Multicast | FF00::/8 | replaces broadcast |
| Anycast | comes from unicast space | nearest of several identical addresses answers |

There is **no broadcast** in IPv6. Multicast (and anycast) replace it.

## Compression rules

1. Omit leading zeros in each group: `0db8` becomes `db8`.
2. Replace one contiguous run of all-zero groups with `::`. Only once per address.

`2001:0db8:0000:0000:0000:0000:0000:0001` becomes `2001:db8::1`.

## EUI-64

Turns a 48-bit MAC into a 64-bit interface ID: split the MAC in half, insert `FFFE` in the middle, flip the 7th bit of the first byte (U/L bit). Used for SLAAC-derived interface IDs.

## Address assignment methods

| Method | How it works |
|---|---|
| SLAAC | Router advertisement (RA) gives prefix, host builds its own interface ID (EUI-64 or random) |
| Stateless DHCPv6 | SLAAC for the address, DHCPv6 only for extra options (DNS, etc.) |
| Stateful DHCPv6 | DHCPv6 assigns the full address, like DHCPv4 |

## Common prefix lengths

| Prefix | Use |
|---|---|
| /64 | standard LAN subnet, always for SLAAC |
| /127 | point-to-point link (RFC 6164) |
| /48 | typical site allocation from an ISP |
