---
title: "Chapter 3, Part 1 – Phases of an IoT System"
description: "The five phases of an IoT system from data collection to delivery, and the types of attacks targeting each phase."
---

## Phases of an IoT System

The IoT requires **five phases**, from data collection to data delivery to end users on or off demand.

---

## Phase I: Data Collection / Acquisition / Perception

- Collect or acquire data from devices or things.
- Based on the characteristics of the thing, different types of data collectors are used.
- The thing may be a **static body** (body sensors or RFID tags) or a **dynamic vehicle** (sensors and chips).

## Phase II: Storage

- Data collected in Phase I must be stored.
- If the thing has its own local memory, data can be stored locally.
- Generally, IoT components have **low memory and low processing capabilities**.
- The **cloud** takes over storage responsibility in the case of stateless devices.

## Phase III: Intelligent Processing

- The IoT analyses data stored in cloud Data Centers (DCs) and provides intelligent services in hard real time.
- As well as analysing and responding to queries, the IoT also **controls things**.
- There is no discrimination between a boot and a bot — the IoT offers intelligent processing and control services to all things equally.

## Phase IV: Data Transmission

Data transmission occurs in **all phases**:

- From sensors, RFID tags, or chips → to DCs
- From DCs → to processing units
- From processors → to controllers, devices, or end users

## Phase V: Delivery

- Delivery of processed data to things **on time, without errors or alteration** is a sensitive task that must always be carried out.

---

## Phase Attacks

Each phase is exposed to different attacks. The major concerns in the **data perception phase** are:

- Data leakage
- Data sovereignty
- Data breach
- Authentication failures

> Attacks exist across all five phases — from the moment data is sensed to the moment it reaches the end user.
