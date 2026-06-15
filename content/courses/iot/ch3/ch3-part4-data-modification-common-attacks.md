---
title: "Chapter 3, Part 4 – Data Modification and Common IoT Attacks"
date: 2026-06-15
description: "Modification of sensitive data, eavesdropping, rogue access points, privilege escalation, and brute-force attacks in IoT."
tags: [iot, security, data-modification, eavesdropping, privilege-escalation, brute-force, chapter3]
toc: true
weight: 4
---

## Modification of Sensitive Data

> Data can be captured during transit, modified, and then forwarded to the intended
> node.

The entire message does not always need to be changed. Sometimes changing one
field is enough to cause harm.

For example:

- A medical reading may be modified before reaching a hospital server.
- Traffic information may be delayed or changed.
- A control command sent to an actuator may be altered.

---

## Three Modification Types

| Type | Meaning | Example Impact |
|---|---|---|
| Content modification | Part of the information is changed | A sensor value is changed from normal to critical |
| Sequence modification | Data is delivered in the wrong order | The receiver reconstructs the message incorrectly |
| Time modification | Old data is delayed or replayed later | The system acts on outdated information |

These attacks affect integrity because the receiver can no longer trust that the
message is correct.

---

## Prevention Ideas

Common prevention methods include:

- End-to-end encryption.
- Message authentication codes.
- Digital signatures.
- Sequence numbers.
- Timestamps.
- Replay protection.
- Firmware and software updates.

Encryption helps protect confidentiality, while authentication and integrity checks
help detect tampering.

---

## Eavesdropping

Eavesdropping means secretly listening to communication to steal data.

In IoT, stolen data may include:

- Sensor readings.
- Device identifiers.
- Commands.
- Credentials.
- Location information.
- Personal or health data.

The attacker does not need to change data for eavesdropping to be harmful. Reading
sensitive data is enough to break confidentiality.

---

## Rogue Access Point

A rogue access point is a fake network access point that tricks users or IoT
devices into connecting.

After a victim connects, traffic may pass through the attacker's device. This can
expose logins, sensor data, commands, or private information.

Useful defenses include:

- Strong Wi-Fi authentication.
- Avoiding unknown networks.
- Certificate validation.
- VPN use where appropriate.
- Network segmentation.
- Monitoring for duplicate or suspicious network names.

---

## Privilege Escalation

Privilege escalation means gaining more access rights than allowed.

| Type | Meaning |
|---|---|
| Horizontal escalation | Moving to another device or account with the same privilege level |
| Vertical escalation | Gaining higher privilege on the same device or system |

Common causes include:

- Default or shared credentials.
- Unpatched vulnerabilities.
- Misconfigured permissions.
- Insecure update mechanisms.
- Weak access control.
- Lack of network segmentation.

Possible impacts include data theft, malware installation, botnet participation,
service disruption, and hiding attacker activity.

---

## Brute-Force Attack

A brute-force attack tries many password guesses until access is obtained.

IoT devices are often vulnerable because some users keep default or weak
passwords. Attackers may also use dictionaries or common password combinations.

Defenses include:

- Changing default credentials immediately.
- Strong unique passwords.
- Account lockout or throttling.
- Multi-factor authentication where possible.
- Disabling unnecessary remote access.
- Monitoring failed login attempts.
- Keeping firmware updated.

---

## Summary

| Attack | Main Security Property Affected |
|---|---|
| Data modification | Integrity |
| Eavesdropping | Confidentiality |
| Rogue access point | Confidentiality and authentication |
| Privilege escalation | Authorization and access control |
| Brute-force | Authentication |
