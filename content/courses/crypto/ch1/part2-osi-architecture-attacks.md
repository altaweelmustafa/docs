---
title: "Chapter 1, Part 2 – The OSI Security Architecture & Security Attacks"
date: 2026-08-09
description: "X.800's three core concepts (attacks, mechanisms, services), threats vs. attacks, and the passive/active attack split."
tags: [information-security, osi-architecture, attacks, chapter1]
toc: true
weight: 2
---

## Why a formal architecture?

Security managers need a systematic way to define requirements and evaluate products — that's hard in a single machine, and even harder across networks. ITU-T Recommendation **X.800 (Security Architecture for OSI)** gives that systematic approach, organized around three core concepts.

| Concept | Definition |
|---|---|
| **Security attack** | Any action that compromises the security of information owned by an organization. |
| **Security mechanism** | A process (or device) designed to detect, prevent, or recover from an attack. |
| **Security service** | A processing/communication service that enhances security; it counters attacks by using one or more mechanisms. |

Think of it as: **attacks** are the threat, **mechanisms** are the tools, **services** are the goals the tools are assembled to achieve.

---

## Threat vs. attack

- **Threat** — a *potential* for a security violation: a circumstance, capability, action, or event that *could* breach security. A possible danger that might exploit a vulnerability.
- **Attack** — an *actual* assault: a deliberate, intelligent attempt to evade security services and violate a security policy.

A threat is the possibility; an attack is the realization of that possibility.

---

## Passive vs. active attacks

X.800 and RFC 4949 both classify attacks along this axis:

| | Passive attack | Active attack |
|---|---|---|
| **Effect on system resources** | None — the attacker only observes/learns | Alters system resources or their operation |
| **Detectability** | Very hard to detect (no data is changed) | Easier to detect, but hard to prevent outright |
| **Defense strategy** | Prevention (e.g. encryption) | Detection + recovery |

### Passive attacks

- **Release of message contents** — an opponent simply reads a sensitive email, call, or file.
- **Traffic analysis** — even if content is encrypted, an opponent can still observe *who* is talking to *whom*, how often, and for how long, and infer something from that pattern.

### Active attacks

Four categories:

- **Masquerade** — one entity pretends to be another (often combined with another active attack, e.g. replaying a captured authentication sequence to gain elevated privileges).
- **Replay** — capturing a legitimate data unit and retransmitting it later to cause an unauthorized effect.
- **Data modification** — part of a legitimate message is altered, or messages are reordered/delayed, to produce an unauthorized effect.
- **Denial of service (DoS)** — prevents or degrades normal use of communication facilities, either against a specific target or by overwhelming/disabling an entire network.

> Exam sentence: passive attacks are hard to detect but easy to prevent (encryption); active attacks are hard to prevent but the goal shifts to detection and recovery.

---

## What to remember for the exam

X.800 organizes the field around attacks, mechanisms, and services. A threat is a potential danger; an attack is the realized act. Attacks split into passive (eavesdropping, traffic analysis — prevent them) and active (masquerade, replay, modification, DoS — detect and recover from them).

---

## Exam check

1. Define security attack, security mechanism, and security service, and explain how they relate.
2. What's the difference between a threat and an attack?
3. List the four types of active attack and give one real-world example of each.
