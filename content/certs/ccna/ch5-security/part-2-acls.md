---
title: "Ch5, Part 2 - Access Control Lists"
date: 2026-08-14
description: "Standard vs extended ACLs, number ranges, wildcard masks, placement rules, config syntax."
tags: [ccna, acl, chapter5]
toc: true
weight: 2
---

## Standard vs extended

| Type | Number range | Filters on | Placement |
|---|---|---|---|
| Standard | 1-99, 1300-1999 | source IP only | as close to the **destination** as possible |
| Extended | 100-199, 2000-2699 | source, destination, protocol, port | as close to the **source** as possible |

Named ACLs work the same way, just with a name instead of a number, and let you edit/delete individual lines (numbered ACLs pre-2000s IOS required full re-entry, modern IOS supports sequence numbers for both).

## Core rules (these get tested directly)

1. Processed top-down, first match wins, stops evaluating after a match.
2. Implicit `deny any` (or `deny ip any any` on extended) at the end of every ACL, even though it's invisible in the config.
3. An ACL with no entries permits everything (rare trick question: an ACL that only has deny statements blocks everything, because of the implicit deny).
4. Applied to an interface with a direction: `in` or `out`, relative to the router.
5. One ACL per protocol, per direction, per interface.

## Wildcard masks

Wildcard = inverse of a subnet mask. `0` = must match, `1` = don't care.

| Subnet mask | Wildcard mask |
|---|---|
| 255.255.255.0 | 0.0.0.255 |
| 255.255.255.192 | 0.0.0.63 |
| 255.255.255.255 | 0.0.0.0 (host, exact match) |
| 0.0.0.0 | 255.255.255.255 (any) |

`host 10.1.1.1` is shorthand for `10.1.1.1 0.0.0.0`. `any` is shorthand for `0.0.0.0 255.255.255.255`.

## Config syntax

```
! standard
access-list 10 permit 192.168.1.0 0.0.0.255
interface g0/1
 ip access-group 10 out

! extended
access-list 110 permit tcp 192.168.1.0 0.0.0.255 any eq 443
access-list 110 deny ip any any
interface g0/0
 ip access-group 110 in
```

## Verification

`show access-lists`, `show ip interface` (shows which ACLs are applied and in which direction).
