---
title: "Chapter 1, Part 4 – IoT Deployment Levels 1–3"
description: "IoT system components, levels 1 through 3 — architecture, strengths, limitations, and wireless interference solutions including CSMA/CA."
---

## IoT System Components (Common to All Levels)

Every IoT system — regardless of level — is built from these blocks:

- **Device** — the physical thing that senses or actuates
- **Resource** — software on the device for accessing sensors, storing data, and networking
- **Controller Service** — runs natively on the device, sends data to the web service and receives commands from the application
- **Database** — local or cloud storage for device-generated data
- **Web Service** — the link between device, app, database, and analysis components (REST or WebSocket)
- **Analysis Component** — processes raw IoT data into results the user can understand
- **Application** — the user-facing interface for monitoring and control

---

## Level 1 — Single Node, Everything Local

A single device handles **everything**: sensing, actuation, local storage, local analysis, and the application — with no cloud involved.

- Suitable for low-cost, low-complexity solutions where data is small and analysis is simple
- Works fully offline with no network dependency
- **Example:** Home automation — a motion sensor detects movement and a local controller turns on a light

**Strengths:**

- Full autonomy — works even without internet
- Very low latency — no network round trips
- Data privacy — everything stays on-device
- Energy efficient and low cost (no cloud infrastructure)

**Limitations:**

- Single point of failure — if the device fails, everything fails
- No remote access — can only be controlled within the local network
- Limited processing power and storage
- Interoperability issues — local devices may use different protocols (Zigbee, BLE) which can collide

---

### Wireless Interference Solutions in Level 1

When multiple devices share the same wireless medium, collisions happen. Three main approaches:

**1. BLE Frequency Hopping**
BLE does not stay on one frequency — it continuously hops between channels. If one channel is noisy, it moves to another automatically. This avoids sustained interference from other devices on the same frequency.

**2. Zigbee Low Data Rate**
Zigbee sends small, slow packets. This means short transmission windows, which reduces the chance of two devices transmitting at the same time and colliding.

**3. CSMA/CA — Carrier Sense Multiple Access with Collision Avoidance**
Used when multiple devices share the same protocol and medium. Instead of transmitting immediately, a device follows this process:

```
Step 1 — Carrier Sense (Listen)
  Device checks if the channel is idle or busy.
  If busy → wait and keep checking.
  If idle → proceed.

Step 2 — Wait IFS (Inter Frame Space)
  Even if idle, device waits a short fixed time.
  Ensures fairness so no one jumps in immediately.

Step 3 — Random Backoff
  Device picks a random wait time within a contention window.
  Backoff Time = R × Slot Time
  Random delay reduces chance of two devices colliding.

Step 4 — Send RTS (Request To Send)
  Device sends a small control packet: "I want to send."

Step 5 — Wait for CTS (Clear To Send)
  Receiver replies CTS if it is ready.
  If CTS received → channel is reserved, proceed.
  If no CTS → assume failure, retry.

Step 6 — Send Data Frame
  Device transmits the actual data packet.

Step 7 — Wait for ACK
  Receiver sends ACK if data arrived correctly.
  If ACK received → success.
  If no ACK → failure.

Step 8 — Exponential Backoff & Retry
  On failure: K = K + 1, backoff range increases.
  If K reaches max → transmission aborted.
  Otherwise → repeat from backoff step.
```

> **Important:** These solutions reduce interference but do **not** solve the interoperability problem — devices still speak different protocols (Zigbee ≠ BLE). A unified platform or gateway is needed for true interoperability.

---

## Level 2 — Single Node, Cloud Storage

A single logical node performs sensing and **simple local analysis**. Data is sent to the cloud for storage, and the application is cloud-based. The node may have one or many sensors but acts as one logical unit.

- Local processing is simple — no heavy computation at the edge
- Cloud provides persistent storage and remote access
- Architecture split:
  - **Local layer:** device + controller service (real-time control)
  - **Cloud layer:** database + web service + application (storage and visualization)
- **Example:** Smart irrigation — sensors measure soil moisture locally, controller decides when to water, data stored in cloud for remote monitoring and scheduling

**Strengths:**

- Low-latency local decisions — controller acts immediately without waiting for cloud
- System keeps working for basic functions even if cloud connection drops
- Scalable storage — cloud removes memory limits of embedded devices
- Clean separation: real-time control at edge, storage and visualization in cloud

**Limitations:**

- Only simple analysis at the edge — no ML or complex algorithms
- Advanced features (trend detection, historical analysis) still require cloud
- Synchronization issues — local data and cloud data can go out of sync during network failure
- Communication between node and cloud is an attack surface (requires TLS, strong auth)
- Single logical node = bottleneck if many sensors feed into it

---

## Level 3 — Cloud-Based Processing & Control

Multiple sensors send data through a **gateway** to the cloud. The cloud handles all storage, analysis, and decision-making. The application (web/mobile dashboard) runs on cloud.

- The gateway aggregates data from multiple devices before sending to cloud
- Suitable for **big data** and **computationally intensive** workloads
- Supports ML, AI, pattern recognition, predictive analytics on large datasets
- **Example:** Package tracking system — thousands of sensors generate continuous data; cloud analyzes routes, delays, and patterns centrally

**Strengths:**

- Powerful computation — cloud has unlimited resources for complex algorithms
- Centralized global view — all data in one place enables system-wide decisions
- Highly scalable — add more devices without changing local hardware
- Long-term historical analysis and predictive insights

**Limitations:**

- Fully cloud-dependent — any network failure stops the entire system
- High latency — control commands must travel to cloud and back
- High communication overhead — all data transmitted continuously to cloud
- Single point of failure — centralized architecture
- Increased security exposure — continuous data transmission to cloud

---

## Summary: Levels 1–3

| Level | Processing     | Storage | App   | Key Idea                                         |
| ----- | -------------- | ------- | ----- | ------------------------------------------------ |
| **1** | Local          | Local   | Local | One device does everything offline               |
| **2** | Local (simple) | Cloud   | Cloud | Edge control + cloud storage/access              |
| **3** | Cloud          | Cloud   | Cloud | Gateway forwards all data; cloud does everything |
