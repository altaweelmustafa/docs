---
title: "Chapter 1, Part 11 – IoT Level-5"
date: 2026-05-31
description: "Level-5 IoT systems: wireless sensor networks with multiple end nodes and a coordinator — forest fire detection example and MATLAB simulation."
tags: [iot, levels, level5, wsn, coordinator, matlab, chapter1]
toc: true
weight: 11
---

## What Is IoT Level-5?

> A Level-5 IoT system has **multiple end nodes** and **one coordinator node**.
> End nodes perform sensing and/or actuation. The coordinator node collects data
> from the end nodes and forwards it to the cloud. Data is stored and analyzed in
> the cloud, and the application is cloud-based.

Level-5 is the standard **Wireless Sensor Network (WSN)** deployment template.
Rather than each node communicating independently with the cloud, all end nodes
report to a single coordinator that handles the upward data transmission.

**Best for:** WSN-based solutions with large data and computationally intensive
cloud analysis requirements.

---

## Architecture

```
[Local]                                     [Cloud]
Observer                    App                     Observer
 Node ◄─────────────────────────────────────────────► Node
              REST/WebSocket Communication
                ▼              ▼              ▼
           Controller    Controller      Controller
            Service       Service         Service
              ▼              ▼              ▼
           Resource       Resource       Resource     REST/WebSocket ◄──► Analytics
              ▼              ▼              ▼            Services          Component
           Endpoint       Endpoint    Coordinator ◄──────────────────── Database
            Device         Device       Device

● Routers/End Points ──► Coordinator ──────────────► Cloud Storage & Analysis
```

End nodes communicate with the coordinator locally (often over Zigbee, Z-Wave, or
LoRa). The coordinator handles the single uplink to the cloud.

---

## Application: Forest Fire Detection

Sensor nodes distributed throughout a forest continuously monitor environmental
conditions.

**How it works:**

- Each end node measures temperature, humidity, smoke density, and other
  fire-risk indicators.
- The coordinator node gathers readings from all end nodes periodically.
- The aggregated dataset is transmitted to the cloud for storage.
- Cloud analytics runs predictive models on the data to assess fire risk and
  forecast potential outcomes before a fire ignites.

**Why Level-5:** Dozens or hundreds of sensors are spread across a large area —
a coordinator is needed to aggregate local data efficiently before the cloud
uplink. Analysis is complex (predictive modeling), so it belongs in the cloud.

---

## MATLAB Simulation

```matlab
time = 0:1:60;

% End nodes generate environmental readings
Temp = 20 + randn(1, length(time));
Hum  = 30 + randn(1, length(time));

% Coordinator aggregates and sends to cloud (xlsx simulates cloud upload)
CoordinatorData = table(time', Temp', Hum', ...
    'VariableNames', {'Time', 'Temp', 'Humidity'});
writetable(CoordinatorData, 'Coordinator.xlsx');

% Cloud analytics reads coordinator data
CloudData = readtable('Coordinator.xlsx');

if mean(CloudData.Temp) > 25
    disp('Alert: High Average Temp');
end
if mean(CloudData.Humidity) > 40
    disp('Alert: High Average Humidity');
end
```

The coordinator aggregates all end-node readings into a single upload. Cloud
analytics evaluates averages across the whole network — not just individual
node readings.

---

