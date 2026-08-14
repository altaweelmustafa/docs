---
title: "Ch5, Part 3 - Layer 2 Security and Wireless Security"
date: 2026-08-14
description: "Port security, DHCP snooping, Dynamic ARP Inspection, WPA/WPA2/WPA3."
tags: [ccna, port-security, dhcp-snooping, dai, wpa, chapter5]
toc: true
weight: 3
---

## Port security

Limits which/how many MAC addresses can use an access port.

```
interface fa0/1
 switchport port-security
 switchport port-security maximum 2
 switchport port-security mac-address sticky
 switchport port-security violation restrict
```

| Violation mode | Traffic from violating MAC | Port state | Logged? |
|---|---|---|---|
| Protect | Dropped | Stays up | No |
| Restrict | Dropped | Stays up | Yes |
| Shutdown (default) | Dropped | Err-disabled | Yes |

Recover an err-disabled port: `shutdown` then `no shutdown`, or configure `errdisable recovery`.

## DHCP snooping

Prevents rogue DHCP servers. Ports are either trusted (uplinks toward the real DHCP server) or untrusted (access ports, default). Untrusted ports can't send DHCP offers/acks, and snooping builds a binding table (MAC to IP to VLAN to port) used by DAI.

```
ip dhcp snooping
ip dhcp snooping vlan 10
interface g0/1
 ip dhcp snooping trust
```

## Dynamic ARP Inspection (DAI)

Prevents ARP spoofing/man-in-the-middle by validating ARP packets against the DHCP snooping binding table. Requires DHCP snooping to be enabled first. Trusted ports (usually the same as DHCP snooping trusted ports) skip inspection.

```
ip arp inspection vlan 10
interface g0/1
 ip arp inspection trust
```

## Wireless security (WPA family)

| Standard | Encryption | Notes |
|---|---|---|
| WEP | RC4 | broken, never the right answer |
| WPA | TKIP | transitional, legacy |
| WPA2 | AES-CCMP | current baseline, PSK or Enterprise (802.1X/RADIUS) |
| WPA3 | AES-GCMP, SAE | current best, protects against offline dictionary attacks (SAE replaces PSK's 4-way handshake weakness) |

PSK = shared passphrase for everyone. Enterprise = individual per-user credentials validated against a RADIUS server, always the answer for "scalable" or "auditable" wireless auth.
