---
title: "Chapter 1, Part 5 – IoT Deployment Levels 4–6"
description: "IoT levels 4 through 6 — multiple observers, WSN coordinator-based networks, centralized cloud control, and a full comparison table of all six levels."
---

## Level 4 — Multiple Nodes, Multiple Observers

Multiple sensor nodes collect data and send it to the cloud. What makes Level 4 distinct is that the **same cloud data is consumed by multiple independent observer nodes**, each performing different analysis or taking different actions.

- Observer nodes can be local (real-time alerts) or cloud-based (analytics dashboards)
- Observers subscribe to cloud data — they don't collect it themselves
- **Example — Noise Monitoring:**
  - Sound sensors in multiple city locations send data to cloud
  - Real-time local observer → detects dangerous noise levels, triggers immediate alert
  - Cloud observer → analyzes long-term noise patterns for city planning
  - Mobile app → lets citizens view noise levels in their area
  - Government system → subscribes to detect violations and enforce regulations

All four consume the **same data**, for completely different purposes.

**Example — Smart City Traffic:**

- Traffic sensors send data to cloud
- Traffic lights system uses it for real-time signal control
- Navigation apps use it for route suggestions
- Emergency services use it to find the fastest path
- City analytics uses it for long-term urban planning

**Strengths:**

- One data source powers many independent systems
- Enables both real-time and long-term analysis simultaneously
- Flexible — new observers can subscribe without changing the sensors

**Limitations:**

- High communication overhead — every sensor communicates frequently and directly
- High energy consumption — especially for battery-powered sensors
- Network congestion — many sensors transmitting at the same time causes collisions
- Poor scalability — flat architecture struggles with very large sensor counts
- No structured organization — sensors have no hierarchy, coordination is messy

---

## Level 5 — Coordinator-Based Sensor Network (WSN)

Level 5 solves Level 4's scalability and energy problems by organizing sensors into **clusters**. Each cluster has a **coordinator node (cluster head)** that collects, aggregates, and forwards data on behalf of its sensors. Sensors do **not** communicate directly with the cloud.

- Based on **Wireless Sensor Networks (WSN)**
- Coordinator removes duplicates, computes averages, and sends a summary only
- Multiple coordinators send their summaries to a gateway, which forwards to cloud
- Suitable for large-scale deployments with big data requirements

### LEACH Protocol

LEACH (Low-Energy Adaptive Clustering Hierarchy) is the standard clustering protocol for WSN-based IoT:

- Sensors are grouped into clusters
- Each cluster elects a **Cluster Head** (coordinator)
- Cluster Head: collects data from members → aggregates → sends to cloud/base station
- Cluster Heads rotate over time to distribute energy load

**Example — Forest Fire Detection:**

```
Sensors (temp, smoke, humidity)
    → send raw data to local Cluster Head
        → Cluster Head removes duplicates, calculates averages
            → sends summary to gateway
                → gateway sends to cloud
                    → cloud runs fire prediction model, sends alerts
```

**Strengths (solving Level 4 problems):**

- Fewer transmissions → significantly less energy consumption
- Data aggregation → less redundant data sent to cloud
- Structured hierarchical network → better organized, easier to manage
- Better scalability — adding more sensors only adds to clusters, not to cloud load

**Limitations:**

- Coordinator is a single point of failure for its cluster
- Cluster Head consumes more energy than regular nodes (heavier workload)
- Cluster management adds complexity
- Coordinator bottleneck if too many sensors in one cluster

---

## Level 6 — Centralized Cloud Controller

Multiple independent end nodes send data directly to the cloud (no coordinator hierarchy like Level 5). The cloud stores, analyzes, and makes decisions — and a **centralized cloud controller** is aware of all nodes and actively sends control commands back to them.

- Global intelligence — the controller sees the entire system, not just one cluster
- Cross-device correlation — decisions can be based on data from multiple locations
- Hierarchical control: local controllers handle immediate actions + central cloud controller handles global decisions
- The analytics component analyzes data and stores results; the application visualizes them
- **Example:** Smart grid — cloud controller monitors power consumption across an entire city and sends load-balancing commands to individual nodes

**How Level 6 differs from Level 5:**

- Level 5 focuses on **how sensors are organized** (clustered WSN)
- Level 6 focuses on **global control** — the cloud actively manages and commands all nodes

**Strengths:**

- True global intelligence — not limited to local or per-cluster decisions
- Cross-device and cross-location data correlation
- Centralized monitoring, management, and decision support
- Scales to very large systems with hierarchical control structure

**Limitations:**

- Fully cloud-dependent — no cloud = no control
- Highest latency of all levels (all commands route through cloud)
- Largest attack surface — maximum data exposure in transit
- Most complex architecture to implement and secure

---

## Full Comparison Table — All 6 Levels

| Level | Processing                | Storage | App   | Key Idea                             | Best For                             | Example               |
| ----- | ------------------------- | ------- | ----- | ------------------------------------ | ------------------------------------ | --------------------- |
| **1** | Local                     | Local   | Local | Single node, fully offline           | Simple, low-cost, local-only         | Home automation       |
| **2** | Local (simple)            | Cloud   | Cloud | Edge control + cloud storage         | Remote access with local decisions   | Smart irrigation      |
| **3** | Cloud                     | Cloud   | Cloud | Gateway → all processing in cloud    | Big data, ML, heavy analytics        | Package tracking      |
| **4** | Cloud                     | Cloud   | Cloud | Multiple observers consume same data | Multi-purpose data reuse             | Smart city traffic    |
| **5** | Coordinator + Cloud       | Cloud   | Cloud | WSN clusters, coordinator aggregates | Large sensor networks, energy saving | Forest fire detection |
| **6** | Cloud (global controller) | Cloud   | Cloud | Cloud controller commands all nodes  | Enterprise-scale global control      | Smart grid            |
