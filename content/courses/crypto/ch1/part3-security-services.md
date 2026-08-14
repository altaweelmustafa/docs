---
title: "Chapter 1, Part 3 – Security Services"
date: 2026-08-09
description: "Authentication, access control, data confidentiality, data integrity, non-repudiation, and the availability service, as defined in X.800."
tags: [information-security, security-services, chapter1]
toc: true
weight: 3
---

## Authentication

Concerned with assuring a communication is genuinely from who it claims to be from.

- **Single message case**: assures the recipient the message really came from the claimed source.
- **Ongoing connection case**: assures both entities are authentic *and* that a third party can't hijack the connection to masquerade as one of the legitimate parties.

X.800 defines two specific authentication services:

| Service | What it covers |
|---|---|
| **Peer entity authentication** | Corroborates the identity of a peer at connection setup or during data transfer. Guards against masquerade or unauthorized replay of a previous connection. |
| **Data origin authentication** | Corroborates the source of a single data unit. Does *not* protect against duplication/modification — useful for connectionless applications like email where there's no ongoing session. |

---

## Access control

The ability to limit and control access to hosts and applications over communication links. Requires that every entity first be **identified/authenticated**, so access rights can be tailored to that specific individual.

---

## Data confidentiality

Protects transmitted data from **passive** attacks, at two possible levels:

- **Content protection** — broadest form protects *all* user data on a connection; narrower forms protect just one message or specific fields.
- **Traffic-flow protection** — hides *that* communication is happening at all: source, destination, frequency, and length of traffic, not just its content.

## Data integrity

Can apply to a stream of messages, one message, or selected fields.

| Type | Covers |
|---|---|
| **Connection-oriented (stream) integrity** | Messages arrive as sent — no duplication, insertion, modification, reordering, or replay. Also covers destruction of data (so it overlaps with denial-of-service protection). |
| **Connectionless integrity** | Deals with individual messages in isolation — generally just protects against modification. |

Because integrity relates to *active* attacks, the emphasis is on **detection** rather than prevention — and ideally automated recovery once a violation is detected.

## Non-repudiation

Prevents either party from denying a transmitted message:

- If a message is sent, the **receiver** can prove the sender really sent it.
- If a message is received, the **sender** can prove the receiver really received it.

## Availability service

Protects a system to ensure it stays available, directly countering denial-of-service attacks. It depends on proper management of system resources, and therefore leans on access control and the other services above.

> Exam sentence: X.800 defines five service families — authentication, access control, confidentiality, integrity, and non-repudiation — plus a dedicated availability service that addresses DoS.

---

## What to remember for the exam

Each security service maps to specific attacks it defends against: authentication → masquerade, access control → unauthorized access, confidentiality → passive attacks, integrity → active/data-modification attacks, non-repudiation → denial of having sent/received, availability → DoS.

---

## Exam check

1. What's the difference between peer entity authentication and data origin authentication?
2. Contrast connection-oriented integrity with connectionless integrity.
3. Explain non-repudiation with a concrete sender/receiver example.
