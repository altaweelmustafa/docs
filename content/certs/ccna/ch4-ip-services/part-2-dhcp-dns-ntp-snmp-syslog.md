---
title: "Ch4, Part 2 - DHCP, DNS, NTP, SNMP, Syslog"
date: 2026-08-14
description: "DORA process, DHCP relay, DNS, NTP stratum, SNMP versions, syslog severity levels."
tags: [ccna, dhcp, dns, ntp, snmp, syslog, chapter4]
toc: true
weight: 2
---

## DHCP

- Process: **D**iscover &rarr; **O**ffer &rarr; **R**equest &rarr; **A**cknowledge (DORA).
- Ports: client 68, server 67 (UDP).
- DHCP relay: `ip helper-address <dhcp-server-ip>` on the router interface facing the client subnet, forwards broadcast DHCP requests as unicast to a remote server.

## DNS

- Port 53 (TCP and UDP, UDP for normal queries, TCP for zone transfers or large responses).
- Resolves names to IP addresses. Know the difference between a recursive query (resolver does all the work) and iterative (client walks the chain itself), though CCNA depth here is shallow.

## NTP

- Port 123 (UDP).
- Stratum levels: stratum 0 = reference clock (atomic/GPS), stratum 1 = directly connected to stratum 0, each additional hop increases stratum by 1. Lower stratum = more authoritative.

## SNMP

| Version | Security |
|---|---|
| v1 | community string, plaintext, weak |
| v2c | community string, plaintext, adds bulk retrieval (GetBulk) |
| v3 | username/password + optional encryption, actual security |

- Port 161: agent listens for manager requests (Get/Set).
- Port 162: traps sent from agent to manager (unsolicited alerts).

## Syslog severity levels (0 = worst)

| Level | Name |
|---:|---|
| 0 | Emergency |
| 1 | Alert |
| 2 | Critical |
| 3 | Error |
| 4 | Warning |
| 5 | Notice |
| 6 | Informational |
| 7 | Debug |

Mnemonic: "Every Awesome Cisco Engineer Will Need Ice cream Daily." Port 514 (UDP).
