---
title: "Ch5, Part 1 - Security Concepts and AAA"
date: 2026-08-14
description: "CIA triad, threat terms, AAA, TACACS+ vs RADIUS."
tags: [ccna, security, aaa, tacacs, radius, chapter5]
toc: true
weight: 1
---

## CIA triad

| Principle | Meaning |
|---|---|
| Confidentiality | Only authorized parties can read data (encryption). |
| Integrity | Data isn't tampered with (hashing, checksums). |
| Availability | Systems stay accessible (redundancy, DoS mitigation). |

## Threat terminology

| Term | Meaning |
|---|---|
| Vulnerability | a weakness in a system |
| Exploit | the mechanism used to take advantage of a vulnerability |
| Threat | anything that could exploit a vulnerability |
| Mitigation | the action taken to reduce risk |

Common attack types to recognize by name: spoofing (fake source), reflection/amplification (DDoS via third party), man-in-the-middle, social engineering (phishing).

## AAA

| Component | Question it answers |
|---|---|
| Authentication | Who are you? |
| Authorization | What are you allowed to do? |
| Accounting | What did you actually do? (logging) |

## TACACS+ vs RADIUS

| Feature | TACACS+ | RADIUS |
|---|---|---|
| Owner | Cisco proprietary | Open standard |
| Transport | TCP, port 49 | UDP, ports 1812/1813 |
| Encryption | Encrypts entire packet | Encrypts only the password |
| AAA | Separates authentication, authorization, accounting | Combines authentication + authorization |
| Typical use | Device administration (CLI access) | Network access (802.1X, VPN) |

Exam shortcut: "device admin" leans TACACS+, "network/user access" leans RADIUS.

## Local vs AAA

Local: `username admin secret cisco123`, `line vty 0 4` / `login local`. Simple but doesn't scale, no centralized accounting.

AAA server-based: centralizes auth for many devices, config uses `aaa new-model` plus a defined method list pointing to TACACS+/RADIUS server group, with local as fallback.
