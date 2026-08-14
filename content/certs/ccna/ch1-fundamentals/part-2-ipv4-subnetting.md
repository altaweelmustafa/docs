---
title: "Ch1, Part 2 - IPv4 Addressing and Subnetting"
date: 2026-08-14
description: "Private ranges, subnetting shortcut method, VLSM, the CIDR table you must memorize."
tags: [ccna, ipv4, subnetting, vlsm, chapter1]
toc: true
weight: 2
---

## Private ranges (RFC 1918) and special addresses

| Range | Class | Notes |
|---|---|---|
| 10.0.0.0/8 | A | large orgs |
| 172.16.0.0/12 | B | 172.16.0.0 to 172.31.255.255 |
| 192.168.0.0/16 | C | small networks |
| 169.254.0.0/16 | - | APIPA, self-assigned when DHCP fails |
| 127.0.0.0/8 | - | loopback |

## Default classful masks

| Class | Range (1st octet) | Default mask |
|---|---|---|
| A | 1-126 | /8 (255.0.0.0) |
| B | 128-191 | /16 (255.255.0.0) |
| C | 192-223 | /24 (255.255.255.0) |

127 is reserved for loopback. Classful addressing is legacy, but the exam still expects you to recognize it.

## The CIDR table (memorize this cold)

| CIDR | Mask | Block size | Usable hosts |
|---|---|---|---:|---:|
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /29 | 255.255.255.248 | 8 | 6 |
| /30 | 255.255.255.252 | 4 | 2 |
| /31 | 255.255.255.254 | 2 | 2 (point-to-point, RFC 3021) |
| /32 | 255.255.255.255 | 1 | host route |

## Subnetting shortcut (the only method you need)

1. Block size = 256 minus the interesting octet's mask value.
2. Subnets increase by the block size in that octet.
3. Broadcast = next subnet address minus 1.
4. Usable hosts = subnet address + 1 to broadcast - 1.

Example: 192.168.10.0/27, mask octet = 224, block size = 32.
Subnets: .0, .32, .64, .96, .128, .160, .192, .224.
For .64: usable = .65-.94, broadcast = .95.

Do this in your head, not on paper. Speed here is the single highest-leverage skill for the whole exam.

## VLSM

Variable Length Subnet Masking: use different mask sizes across the same major network so you don't waste address space. Rule of thumb: assign the largest subnet requirement first, then work down to the smallest (point-to-point links get /30 or /31).

## Wildcard masks

Wildcard mask = 255.255.255.255 minus the subnet mask. Used in ACLs and OSPF `network` statements. `0` bits mean "must match," `1` bits mean "don't care." This is the #1 confusion point on the exam, don't mix it up with the subnet mask itself.
