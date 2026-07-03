---
title: "Chapter 5, Part 5 – Location Systems"
date: 2026-07-03
description: "Positioning nodes, landmarks, GPS, WiFi-based location, and logical positioning."
tags: [distributed-systems, location-systems, gps, wifi, chapter5]
toc: true
weight: 5
---

## Why location matters

In large-scale distributed systems, nodes can be spread across wide areas.

We often need proximity or distance for:

- choosing nearest server,
- routing efficiently,
- placing replicas,
- reducing latency,
- location-aware services.

Before using proximity, we need to determine node location.

---

## Computing position with landmarks

Observation:

A node `P` needs `d + 1` landmarks to compute its position in a `d`-dimensional space.

Examples:

| Space | Dimensions | Landmarks needed |
|---|---:|---:|
| Line | 1D | 2 |
| Plane | 2D | 3 |
| 3D space | 3D | 4 |

For two-dimensional space, `P` solves three equations for unknown `(xP, yP)`.

---

## GPS

GPS uses satellites as landmarks.

Assuming satellite clocks are accurate and synchronized:

- signal takes time to reach receiver,
- receiver clock is not synchronized with satellites,
- receiver must solve for position and clock error.

Important observation:

> GPS needs 4 satellites because there are 4 unknowns: three position coordinates plus receiver clock offset.

---

## WiFi-based location services

Basic idea:

1. Have a database of known access points (APs) and coordinates.
2. Estimate distance to APs.
3. With 3 detected APs, compute position in 2D.

---

## War driving

War driving is a way to build an AP-location database.

Method:

- use WiFi-enabled device plus GPS receiver,
- move through an area,
- record observed APs and GPS coordinates.

To estimate AP location:

If AP was detected at `N` known locations `x1, x2, ..., xN`, compute centroid:

```text
xAP = (Σ xi) / N
```

Problems:

- GPS detection points have limited accuracy,
- AP transmission range is nonuniform,
- number of samples may be too low.

---

## Logical positioning of nodes

Sometimes measured latency to landmarks is used as a distance estimate.

Problems:

- measured latencies fluctuate,
- computed distances can be inconsistent,
- network distance is not always physical distance.

Example issue:

In a 1D space, inconsistent distances may make it impossible to place nodes in a way that satisfies all measured latencies.

---

## Exam check

1. Why do distributed systems need node location/proximity?
2. How many landmarks are needed in `d` dimensions?
3. Why does GPS need 4 satellites?
4. How does WiFi-based positioning work?
5. What problems happen when using latency as distance?
