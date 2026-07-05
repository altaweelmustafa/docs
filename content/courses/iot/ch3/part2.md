---
title: "Chapter 3, Part 2 – Data Leakage, Sovereignty, Loss & Authentication"
description: "Data leakage definition, causes, consequences, and countermeasures, plus data sovereignty, data loss, and data authentication in IoT."
---

## Data Leakage / Breach

Export of unauthorized data or information to an unintended destination is **data leakage**.

- Can be internal or external, intentional or unintentional, authorized or malicious, involving hardware or software.
- Generally done by a dishonest or dissatisfied employee.
- As cloud data moves from one tenant to several others, there is serious risk of leakage.
- Data leakage is a serious threat to **reliability**.
- Severity can be reduced by **DLP (Data Leakage Prevention)**.

---

## How Data Leakage Happens

- **Weak security practices** — many IoT devices prioritize convenience over security (weak encryption, default configurations, default passwords).
- **Exploited vulnerabilities** — attackers exploit weaknesses in network protocols for malicious activities.
- **Accidental actions** — human errors, device misconfiguration, or accidental sharing of information.
- **Privacy breaches** — personal information such as home habits in smart home systems, or health data in smart health systems, can be exposed.

---

## Consequences of Data Leakage

- **Privacy breaches** — sensitive personal data exposed.
- **Disrupted operations** — data breaches in industrial IoT systems can disrupt production lines in factories and cause physical harm.

---

## Countermeasures for Data Leakage

- **Strong encryption** — implement strong encryption algorithms for data at rest to make it unreadable to unauthorized users.
- **Regular security updates** — system administrators deal with discovered vulnerabilities through timely patching.
- **Strong authentication** — techniques such as challenge-response or MFA (Multi-Factor Authentication).
- **Network segmentation** — isolate IoT devices on separate networks to minimize the blast radius of a breach.
- **Data minimization** — only collect and deal with necessary data.
- **Awareness** — educate employees and users on how to handle IoT devices properly.

---

## Data Sovereignty

- **Data sovereignty** means that information stored in digital form is subject to the **laws of the country** where it is stored.
- The IoT encompasses all things across the globe and is hence liable to sovereignty issues — data stored in a foreign country may be subject to that country's laws.

---

## Data Loss

Data loss differs from data leakage:

- **Data leakage** — often intentional, a sort of revenge-taking by an employee or insider.
- **Data loss** — losing work **accidentally** due to hardware or software failure, or natural disasters.

---

## Data Authentication

- Data can be perceived from any device at any time — devices can be **forged by intruders**.
- It must be ensured that perceived data are received from **intended or legitimate users only**.
- It is mandatory to verify that data have **not been altered during transit**.
- Data authentication provides **integrity** and **originality**.

### Why Data Authentication Matters

- **Sensor data** — sensing data from industrial equipment must be accurate to build correct decisions.
- **Control commands** — commands sent to actuators or IoT devices (e.g. adjusting a thermostat) must not be modified or tampered with during transmission.
- **Originality** — detecting spoofing attacks by verifying that traffic comes from a legitimate source.

### How to Achieve It

```
MAC = H(Message + Shared Key)
```

Or using **Asymmetric Encryption** for stronger guarantees.

### Considerations

- **Standardization** — IoT lacks a single universal authentication standard, making securing devices complex.
- **Scalability** — the authentication solution must scale to handle a potentially vast number of devices.
- **Resource constraints** — many IoT devices have limited processing power and memory; authentication must be efficient to avoid impacting device performance.
