---
title: "Chapter 1, Part 1 – Cybersecurity, Information Security & Security Objectives"
date: 2026-08-09
description: "How cybersecurity, information security, and network security relate, plus the CIA triad and its extensions."
tags: [information-security, definitions, cia-triad, chapter1]
toc: true
weight: 1
---

## Three overlapping terms

The field uses three related terms, and exam questions like to probe whether you can tell them apart.

| Term | Scope |
|---|---|
| **Cybersecurity** | The broadest term — tools, policies, safeguards, risk management, and technologies used to protect the whole cyberspace environment: devices, people, infrastructure, applications, and stored/transmitted information. |
| **Information security** | Preserving **confidentiality, integrity, and availability** of information — electronic *and* physical (e.g. paper records). Also covers authenticity, accountability, non-repudiation, and reliability. |
| **Network security** | Protecting networks and the services running over them from unauthorized modification, destruction, or disclosure, while making sure the network keeps doing its job correctly. |

Information security and network security are both **subsets of cybersecurity**. In everyday use, "cybersecurity" and "information security" are often used interchangeably — but if a question asks specifically about physical/paper records, the correct umbrella term is information security, not network security.

---

## Security objectives — the CIA triad, extended

The classic three objectives:

- **Confidentiality** — private data isn't disclosed to unauthorized parties. Has two angles:
  - *Data confidentiality*: the information itself stays hidden from unauthorized individuals.
  - *Privacy*: individuals control what is collected/stored about them and who it's shared with.
- **Integrity** — data and systems are changed only in authorized ways. Two angles:
  - *Data integrity*: covers authenticity (the object really is what it claims to be) and non-repudiation (sender/receiver can't later deny having sent/received it).
  - *System integrity*: the system performs its intended function, free of unauthorized manipulation.
- **Availability** — authorized users get timely, reliable access to systems and data.

Beyond the classic triad (Figure 1.1 in the textbook), two more properties are often added to give a fuller picture:

- **Authenticity** — confidence that a transmission, message, or its originator is genuine; users are who they claim to be, and data arrives from a trusted source.
- **Accountability** — actions of an entity can be traced uniquely back to that entity. Supports non-repudiation, deterrence, forensic analysis, and legal action — because we can't yet build systems that are perfectly secure, we need to be able to trace a breach to whoever caused it.

> Exam sentence: the CIA triad (confidentiality, integrity, availability) is the foundation, but a complete picture of "information and network security objectives" also includes authenticity and accountability.

---

## What to remember for the exam

Cybersecurity is the umbrella; information security and network security sit underneath it. The core security objectives are confidentiality, integrity, and availability, extended with authenticity and accountability.

---

## Exam check

1. How do cybersecurity, information security, and network security relate to each other?
2. What are the two sub-concepts under confidentiality? Under integrity?
3. Why are authenticity and accountability added to the classic CIA triad?
