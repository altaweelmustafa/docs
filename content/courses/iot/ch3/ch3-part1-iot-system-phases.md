---
title: "Chapter 3, Part 1 – IoT System Phases"
date: 2026-06-15
description: "The five IoT system phases: data collection, storage, intelligent processing, data transmission, and delivery."
tags: [iot, security, threats, phases, chapter3]
toc: true
weight: 1
---

## What Are the Phases of an IoT System?

> IoT security becomes easier to understand when the system is divided into phases.
> Each phase has a specific role, and each role introduces different security risks.

An IoT system usually moves data through five main phases: collecting data,
storing it, processing it, transmitting it, and delivering the result to users or
other devices.

---

## Phase I: Data Collection / Acquisition / Perception

This phase collects or acquires data from IoT devices and things.

Examples of data collectors include:

- Sensors that collect dynamic readings.
- RFID tags that identify static objects.
- Chips inside vehicles or moving devices.
- Body sensors in healthcare systems.

The main security concern is trust. If the device is fake, compromised, or
misconfigured, the whole IoT system may make decisions based on wrong data.

---

## Phase II: Storage

After data is collected, it must be stored.

Storage may happen in:

- Local memory on the IoT device.
- A nearby gateway or local system.
- A cloud data center.

Many IoT devices have limited memory and low processing power, so cloud storage is
commonly used. This solves the storage problem but introduces new risks such as
unauthorized access, data leakage, and data sovereignty issues.

---

## Phase III: Intelligent Processing

In this phase, the system analyzes stored data and provides smart services.

Processing may include:

- Answering user queries.
- Detecting events or anomalies.
- Controlling connected devices.
- Triggering automatic actions.

The major risk is that wrong or modified data can lead to wrong decisions. In
critical systems such as healthcare, traffic, or factories, this can cause serious
harm.

---

## Phase IV: Data Transmission

Data transmission happens between different parts of the IoT system.

Examples include:

- Sensors, RFID tags, or chips sending data to data centers.
- Data centers sending data to processing units.
- Processors sending commands to controllers, devices, or end users.

Transmission needs confidentiality, integrity, and authentication. Attackers may
try to listen to data, modify it, replay old messages, or pretend to be legitimate
devices.

---

## Phase V: Delivery

Delivery means sending the processed data or control command to the correct
receiver.

A secure delivery process should ensure that:

- Data reaches the intended receiver.
- Data is not changed during delivery.
- Commands arrive on time.
- Legitimate users and devices can still access the service.

---

## Quick Summary

| Phase | Main Purpose | Main Security Concern |
|---|---|---|
| Data collection | Gather data from devices | Fake or compromised data source |
| Storage | Store data locally or in the cloud | Leakage, access control, data loss |
| Processing | Analyze data and control devices | Wrong decisions from manipulated data |
| Transmission | Move data between components | Eavesdropping, modification, replay |
| Delivery | Send final result or command | Delay, alteration, or wrong receiver |
