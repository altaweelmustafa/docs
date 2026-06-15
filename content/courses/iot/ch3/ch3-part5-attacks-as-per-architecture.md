---
title: "Chapter 3, Part 5 – Attacks as per Architecture"
date: 2026-06-15
description: "IoT architecture layers and the attack categories associated with the sensing, network, transport, and application layers."
tags: [iot, security, architecture, layers, attacks, chapter3]
toc: true
weight: 5
---

## Attacks as per Architecture

> IoT has not been confined to one fixed architecture. Different vendors and
> applications may adopt different layers.

A common IoT architecture has four layers:

1. Sensing / perception layer.
2. Network layer.
3. Transport layer.
4. Application layer.

Each layer has different responsibilities and different security risks.

---

## 1. Sensing / Perception Layer

The sensing or perception layer is the lowest layer. It includes sensors, RFID
tags, actuators, and physical devices that collect data from the environment.

Possible attacks include:

- External attacks on devices.
- Witch attack.
- Wormhole and sewage pool attacks.
- Broadcast authentication and flooding attacks.
- Link layer attacks.
- HELLO flooding.
- Selective forwarding.
- Access control attacks.

The main concern is that physical and low-level devices may be exposed,
resource-limited, and easier to compromise.

---

## 2. Network Layer

The network layer connects devices and routes data.

Possible attacks include:

- Routing protocol attacks.
- Address compromise.

If routing is manipulated, data can be delayed, dropped, redirected, or sent
through an attacker-controlled path.

---

## 3. Transport Layer

The transport layer handles communication between endpoints.

Possible attacks include:

- Denial of service.
- Distributed denial of service.
- Masquerade attacks.
- Man-in-the-middle attacks.
- Cross-heterogeneity issues.

The main concern is keeping communication reliable, authenticated, and available.

---

## 4. Application Layer

The application layer provides services to users and organizations.

Possible attacks include:

- Revealing sensitive data.
- Data destruction.
- User authentication attacks.
- Intellectual property attacks.

The main concern is protecting user data, application logic, access control, and
service trust.

---

## Layer Summary

| IoT Layer | Main Role | Example Security Risks |
|---|---|---|
| Sensing / perception | Collect physical-world data | Device compromise, flooding, selective forwarding |
| Network | Route and address communication | Routing attacks, address compromise |
| Transport | Transfer data between endpoints | DoS, DDoS, masquerade, man-in-the-middle |
| Application | Provide user services | Data exposure, authentication attacks, data destruction |

---

## Main Idea

Attacks as per architecture classify threats by where they happen. This makes it
easier to choose the right protection method for each layer instead of treating IoT
security as one general problem.
