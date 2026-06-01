---
title: "Chapter 1, Part 10 – IoT Level-4"
date: 2026-05-31
description: "Level-4 IoT systems: multiple nodes with local analysis, cloud storage, and observer nodes — noise monitoring example and MATLAB simulation."
tags: [iot, levels, level4, cloud, observer, matlab, chapter1]
toc: true
weight: 10
---

## What Is IoT Level-4?

> A Level-4 IoT system has **multiple nodes** that each perform local analysis.
> Data is stored in the cloud. The application is cloud-based. It includes both
> local and cloud-based **observer nodes** that can subscribe to and receive data
> collected in the cloud from IoT devices.

The jump from Level-3 to Level-4 is: **multiple nodes** plus **observer nodes**
that subscribe to cloud data for additional processing, visualization, or
alerting.

**Best for:** Solutions requiring multiple distributed nodes, large data, and
computationally intensive analysis.

---

## Architecture

```
[Local]                                [Cloud]
Observer                    App               Observer
 Node ◄──────────────────────────────────────► Node
         REST/WebSocket Communication
              ▼                    ▼
Controller  Controller      REST Services ◄──► Analytics
 Service     Service              ▼            Component
    ▼           ▼             Database        (IoT Intelligence)
 Resource    Resource
    ▼           ▼
  Device      Device

● Monitoring Nodes ──────────────────────► Cloud Storage
  (perform local analysis)
```

Observer nodes — both local and cloud-based — subscribe to data aggregated in the
cloud and may run additional analytics, dashboards, or trigger actions based on
predefined rules.

---

## Application: Noise Monitoring System

Multiple sound sensor nodes are placed in different physical locations to measure
noise levels across an area — a city district, an industrial site, a concert venue.

**How it works:**

- Each node independently measures and analyzes noise levels locally.
- Noise data from all nodes is transmitted to the cloud for storage.
- Observer nodes subscribe to the cloud data and produce dashboards, regulatory
  reports, or alerts when noise thresholds are exceeded.

**Why Level-4:** Multiple geographically distributed sensors are needed. Local
analysis handles threshold detection per node. Cloud storage and observer nodes
handle cross-site aggregation and reporting.

---

## MATLAB Simulation

```matlab
time = 0:1:60;

% Node 1 - Temperature sensor
Temp1 = 20 + randn(1, length(time));
TempStatus1 = Temp1 > 25;
Node1Data = table(time', Temp1', TempStatus1', ...
    'VariableNames', {'Time', 'Temp', 'TempStatus'});
writetable(Node1Data, 'Node1.xlsx');

% Node 2 - Humidity sensor
Hum2 = 30 + randn(1, length(time));
HumStatus2 = Hum2 > 40;
Node2Data = table(time', Hum2', HumStatus2', ...
    'VariableNames', {'Time', 'Humidity', 'HumStatus'});
writetable(Node2Data, 'Node2.xlsx');

% Cloud observer reads aggregated data and checks alerts
Node1 = readtable('Node1.xlsx');
Node2 = readtable('Node2.xlsx');

if any(Node1.TempStatus)
    disp('High Temp');
end
if any(Node2.HumStatus)
    disp('Node2 Alert: High Humidity');
end
```

Each node writes its own data file (simulating cloud upload). The observer node
reads both and evaluates alerts independently — no direct communication between
Node 1 and Node 2.

---

