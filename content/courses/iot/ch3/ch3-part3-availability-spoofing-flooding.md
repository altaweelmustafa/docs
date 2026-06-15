---
title: "Chapter 3, Part 3 – Availability, Spoofing, and Flooding"
date: 2026-06-15
description: "Availability attacks in IoT, DDoS concepts, spoofing, flooding types, flash crowds, and defensive countermeasures."
tags: [iot, security, availability, spoofing, flooding, ddos, chapter3]
toc: true
weight: 3
---

## Attacks on Availability

> Availability means legitimate users can access the system when needed.

In IoT, availability is critical because many systems control real-world services
such as health monitoring, industrial equipment, transportation, and smart homes.

A major threat to availability is denial of service. In a denial-of-service attack,
the target is overloaded so that normal users cannot access it. When the overload
comes from many distributed devices, it becomes a distributed denial-of-service
attack.

---

## Types of Overload Threats

Availability may be affected by:

- Flooding by attackers.
- Flooding by legitimate users during sudden demand, also called a flash crowd.
- Flooding using spoofed identities.
- Flooding by aggressive legitimate clients.

| Type | Description | Main Difference |
|---|---|---|
| Flooding by attackers | Malicious traffic overloads the server or data center | Traffic is intentionally harmful |
| Flooding by legitimate users | Many real users access the system at the same time | Users are real, but volume is too high |
| Flooding by spoofing | Requests appear to come from fake or forged sources | Source identity is unreliable |
| Aggressive legitimate flooding | Valid clients send too many requests | Client is valid, but behavior is excessive |

---

## Spoofing

Spoofing means pretending to be another device, address, or identity.

In IoT, spoofing is dangerous because many devices automatically trust messages
that appear to come from legitimate sources.

Common examples include:

- IP address spoofing.
- MAC address spoofing.
- Fake device identity.
- Fake access point identity.

The attacker uses false identity information to hide, impersonate another source,
or make the victim process unwanted traffic.

---

## ICMP Spoofing / Ping Flood Concept

A ping flood is a denial-of-service concept where a target receives too many ICMP
echo-request packets. If the target spends too many resources responding,
legitimate traffic may be delayed or blocked.

When many devices are involved, the attack becomes distributed.

**Main impact:** bandwidth, CPU, and memory may be consumed by useless traffic.

---

## UDP Flood Concept

A UDP flood overloads a target by sending a large number of UDP packets. The victim
may waste resources checking ports or generating error responses.

The important security idea is resource exhaustion. The attacker tries to consume
network bandwidth, processing power, or memory so the service becomes slow or
unavailable.

---

## SYN Flood Concept

A SYN flood targets the connection setup process.

The server keeps many half-open connection states. If too many incomplete
connections exist, the connection table can fill up and real users may not be able
to connect.

**Main impact:** legitimate users cannot establish normal sessions.

---

## Reflection and Amplification

Reflection-based attacks abuse third-party systems to send responses toward a
victim. Amplification happens when small requests cause larger responses.

The defensive lesson is that systems should avoid becoming reflectors and should
monitor abnormal traffic patterns.

---

## Why We Cannot Simply Drop All Suspicious Packets

Dropping packets is not always simple because:

- Some systems legitimately communicate with themselves.
- The IP layer sees addresses but not the attacker's intention.
- Fake traffic can look similar to real traffic.
- Protocols follow standard behavior even when abused.

So defense usually requires several techniques together, not one rule only.

---

## Countermeasures

High-level defenses include:

- Ingress and egress filtering.
- Rate limiting.
- Anti-spoofing rules.
- Network monitoring.
- Anomaly detection.
- Keeping devices updated.
- Disabling unnecessary services.
- Using authentication and access control.

These defenses reduce overload and make fake or abnormal traffic easier to detect.
