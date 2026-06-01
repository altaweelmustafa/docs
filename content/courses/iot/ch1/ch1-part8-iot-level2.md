---
title: "Chapter 1, Part 8 – IoT Level-2"
date: 2026-05-31
description: "Level-2 IoT systems: single node with local analysis, cloud storage — architecture and smart irrigation example."
tags: [iot, levels, level2, cloud, chapter1]
toc: true
weight: 8
---

## What Is IoT Level-2?

> A Level-2 IoT system has a **single node** that performs sensing and/or
> actuation and **local analysis**. Data is stored in the cloud and the
> application is cloud-based.

The key difference from Level-1: analysis still happens locally on the device, but
storage moves to the cloud. The application is also cloud-hosted rather than
running on the device.

**Best for:** Solutions where data volumes are large enough to require cloud
storage, but the primary analysis logic is simple enough to run on the local node.

---

## Architecture

```
[Local]                          [Cloud]
                                   App
                                    ▲
REST/WebSocket               REST/WebSocket Communication
Communication                       ▼
     ▼                       REST/WebSocket Services
Controller Service ◄────────────────►
     ▼                                   ▼
  Resource                            Database
     ▼
  Device

● Monitoring Node — performs analysis locally
                  ──────────────────────► Cloud Storage
```

The node does its own analysis. Only the processed or raw data is pushed to cloud
storage. The cloud application reads from that storage and presents results.

---

## Application: Smart Irrigation

A smart irrigation system uses soil moisture sensors to determine water content in
the ground, then controls water flow accordingly.

**How it works:**

- Soil moisture sensors measure water content continuously.
- The local node compares readings against predefined moisture thresholds.
- If soil is too dry, the valve opens to release water.
- A water schedule can also be configured to control irrigation timing.
- All raw sensor data is stored in the cloud for historical records and reporting.

**Why Level-2:** The analysis logic (compare moisture level to threshold → open/close
valve) is simple enough to run locally. But the volume of continuous sensor data
over time is large enough to warrant cloud storage rather than local disk.

---

## Advantages of Smart Irrigation

- Increases crop productivity through precise watering.
- Reduces water consumption by only irrigating when needed.
- Reduces dependence on manual human intervention.
- Requires minimal water resources compared to scheduled fixed-time irrigation.

---

