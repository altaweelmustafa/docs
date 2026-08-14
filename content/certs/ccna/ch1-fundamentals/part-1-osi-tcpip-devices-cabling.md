---
title: "Ch1, Part 1 - OSI/TCP-IP, TCP vs UDP, Devices, Cabling"
date: 2026-08-14
description: "Layer mapping, TCP vs UDP, device roles, cabling types, common interface issues."
tags: [ccna, osi, tcp-udp, cabling, chapter1]
toc: true
weight: 1
---

## OSI to TCP/IP mapping

| OSI layer      | PDU              | TCP/IP layer   | Example              |
| -------------- | ---------------- | -------------- | -------------------- |
| 7 Application  | Data             | Application    | HTTP, DNS, DHCP      |
| 6 Presentation | Data             | Application    | encryption, encoding |
| 5 Session      | Data             | Application    | session control      |
| 4 Transport    | Segment/Datagram | Transport      | TCP, UDP             |
| 3 Network      | Packet           | Internet       | IP, ICMP             |
| 2 Data Link    | Frame            | Network Access | Ethernet, ARP        |
| 1 Physical     | Bit              | Network Access | cabling, signaling   |

Exam trap: know the PDU name per layer (segment/packet/frame/bit) cold, they ask it directly.

## TCP vs UDP

| Feature      | TCP                                   | UDP                          |
| ------------ | ------------------------------------- | ---------------------------- |
| Connection   | Connection-oriented (3-way handshake) | Connectionless               |
| Reliability  | Acknowledged, retransmits             | Best-effort                  |
| Ordering     | Guaranteed                            | Not guaranteed               |
| Flow control | Yes (windowing)                       | No                           |
| Header size  | 20 bytes                              | 8 bytes                      |
| Use case     | HTTP, FTP, SSH                        | DNS, DHCP, SNMP, VoIP, video |

3-way handshake: SYN, SYN-ACK, ACK. Teardown: FIN, ACK, FIN, ACK.

## Device roles (which layer they act on)

| Device   | Layer       | Job                                                   |
| -------- | ----------- | ----------------------------------------------------- |
| Hub      | 1           | Repeats bits, one collision domain                    |
| Switch   | 2 (some L3) | Forwards frames by MAC, one broadcast domain per VLAN |
| Router   | 3           | Forwards packets by IP, separates broadcast domains   |
| Firewall | 3-7         | Policy-based filtering                                |
| AP       | 1-2         | Bridges wireless clients to wired LAN                 |
| WLC      | 2-3         | Centralizes control of lightweight APs                |

## Cabling

| Type              | Max speed            | Max distance         |
| ----------------- | -------------------- | -------------------- |
| Cat5e             | 1 Gbps               | 100 m                |
| Cat6              | 10 Gbps              | 55 m (100m at 1Gbps) |
| Cat6a             | 10 Gbps              | 100 m                |
| Single-mode fiber | very high, long haul | km range             |
| Multimode fiber   | high, short haul     | up to ~550 m         |

- Straight-through: unlike devices (PC to switch).
- Crossover: like devices (switch to switch, PC to PC). Auto-MDIX on modern switches makes this mostly irrelevant now.
- Fiber connectors: LC, SC, ST. Copper: RJ45.

## Common interface issues

| Symptom                      | Meaning                                          |
| ---------------------------- | ------------------------------------------------ |
| CRC errors                   | Bad cable, EMI, duplex mismatch                  |
| Runts                        | Frames smaller than 64 bytes, collisions/bad NIC |
| Giants                       | Frames larger than 1518 bytes                    |
| Late collisions              | Duplex mismatch, cable too long                  |
| Input/output errors climbing | Physical layer problem, check `show interfaces`  |

`show interfaces` and reading counters is a guaranteed exam topic.
