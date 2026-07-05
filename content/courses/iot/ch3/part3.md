---
title: "Chapter 3, Part 3 – Attacks on Availability, Data Modification & Common IoT Attacks"
description: "DDoS and flooding attacks on availability, data modification types, and the three most common IoT attack types: privilege escalation, brute-force, and architecture-based attacks."
---

## Attacks on Availability

**Availability** is one of the primary security goals — intended clients must be able to access services.

**DDoS (Distributed Denial of Service)** is an overload condition caused by a huge number of distributed attackers overwhelming a target.

Types of flooding that cause Data Centers to become unavailable:

- **Flooding by attackers** — malicious traffic overload.
- **Flooding by legitimates (flash crowd)** — too many real users at once.
- **Flooding by spoofing** — forged source addresses used to amplify traffic.
- **Flooding by aggressive legitimates** — legitimate but poorly behaved clients.

### ICMP Spoofing Attack (Ping Flood)

A ping flood is a DoS attack where the attacker overwhelms a target with **ICMP echo-request packets**, making it inaccessible to normal traffic. When the attack comes from multiple devices it becomes a **DDoS**. This type of attack can consume both outgoing and incoming bandwidth.

---

## Modification of Sensitive Data

During transit from sensors, data can be **captured, modified, and forwarded** to the intended node. Complete modification isn't necessary — altering part of the message is enough.

Modification occurs in three ways:

- **Content modification** — part of the information has been altered.
- **Sequence modification** — data delivery order is disrupted, making the message meaningless.
- **Time modification** — could result in a **replay attack**.

> Example: If an ECG report is altered during a telemedicine diagnosis, the patient may lose their life. Similarly, in road traffic, if a congestion or accident alert is not delivered correctly, it could cause another disaster.

---

## Three Common Types of Attack in IoT

### 1. Privilege Escalation Attack

Involves obtaining **unauthorized access or elevated rights** by a malicious insider or external attacker.

Attackers exploit:
- Unpatched bugs in the system
- Misconfiguration
- Inadequate access controls

### 2. Brute-Force Attack

Most IoT device users keep **default or easy-to-remember passwords**, making them easy targets.

Attackers guess passwords using:
- Dictionaries
- Common word combinations

**Mitigation:** Enable robust authentication such as **2FA**, **MFA**, and **zero-trust models**.

### 3. Architecture-Based Attacks

The IoT has not been confined to a single architecture — different vendors and applications adopt their own layers. In general, IoT is assumed to have **four layers**:

- **Perception layer** (sensing layer) — lowest level
- **Network layer**
- **Transmission layer**
- **Application layer**

Each layer has its own set of vulnerabilities and attack surfaces.

---

## External Attacks

- Trustworthiness of the **cloud service provider** is a key concern.
- Organizations offload both sensitive and insensitive data to obtain services, often unaware of where their data will be processed or stored.
- The provider may share data with others, or use it for malicious actions.
