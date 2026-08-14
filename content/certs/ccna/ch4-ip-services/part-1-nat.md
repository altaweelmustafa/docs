---
title: "Ch4, Part 1 - NAT"
date: 2026-08-14
description: "Static NAT, dynamic NAT, PAT, inside/outside local/global terms, config syntax."
tags: [ccna, nat, pat, chapter4]
toc: true
weight: 1
---

## Types

| Type | Mapping | Use case |
|---|---|---|
| Static NAT | 1:1, fixed | server that needs a consistent public IP |
| Dynamic NAT | many-to-many, from a pool | pool of public IPs shared among more private hosts |
| PAT (NAT overload) | many-to-1, differentiated by port | typical home/office internet edge |

## The four address terms (the exam loves testing these)

| Term | Meaning |
|---|---|
| Inside local | private IP of an inside host, as seen inside |
| Inside global | translated (public) IP of an inside host, as seen outside |
| Outside local | how an outside host's IP appears from inside (usually unchanged) |
| Outside global | actual public IP of the outside host |

## Config essentials

```
interface g0/0
 ip nat inside
interface g0/1
 ip nat outside

access-list 1 permit 10.0.0.0 0.255.255.255

ip nat pool MYPOOL 203.0.113.1 203.0.113.10 netmask 255.255.255.0
ip nat inside source list 1 pool MYPOOL overload
```

`overload` is what makes it PAT. Drop it and it's plain dynamic NAT (pool-based, no port translation).

Static NAT: `ip nat inside source static 10.0.0.5 203.0.113.5`

## Verification

`show ip nat translations`, `show ip nat statistics`.
