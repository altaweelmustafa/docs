---
title: "Chapter 1, Part 2 – IoT Protocols"
description: "IoT communication protocols across the link, network, transport, and application layers — including MQTT, CoAP, 6LoWPAN, TCP variants, TLS, and DTLS."
---

## Why Protocols Matter in IoT

IoT protocols are chosen to:

- **Minimize bandwidth** usage
- **Reduce latency**
- **Conserve energy**
- **Provide security** (encryption, authentication, integrity)

---

## Link Layer

Determines how data is physically sent over the medium (copper wire, radio wave).

| Protocol                       | Use Case                           | Notes                                     |
| ------------------------------ | ---------------------------------- | ----------------------------------------- |
| **Wi-Fi (802.11)**             | Home automation, smart buildings   | High speed, higher power consumption      |
| **Zigbee / Thread (802.15.4)** | Smart energy, industrial, lighting | Low power, mesh network, 2.4GHz           |
| **BLE**                        | Wearables, health monitoring       | Low power, short range, pairs with phones |
| **LoRaWAN**                    | Smart agriculture, smart cities    | Long range (km), low power, high latency  |

**Why not just use Wi-Fi everywhere?**

- Range: LoRaWAN covers kilometers, Wi-Fi doesn't
- Power: Wi-Fi consumes far more energy
- Scalability: Wi-Fi struggles with thousands of devices
- Cost: Wi-Fi modules are more expensive
- Interference: Wi-Fi shares crowded 2.4/5 GHz bands

---

## Network Layer

Responsible for routing IP datagrams from source to destination.

### 6LoWPAN

IPv6 over Low-Power Wireless Personal Area Networks — adapts IPv6 for constrained IoT devices.

- **Header Compression** — IPv6 headers are ~40 bytes; 6LoWPAN compresses them significantly using LOWPAN_IPHC.
- **Fragmentation** — IEEE 802.15.4 max packet = 127 bytes; IPv6 needs 1280 bytes minimum → 6LoWPAN fragments and reassembles.
- **Mesh Routing** — uses RPL (Routing Protocol for Low-power and Lossy Networks) for multi-hop communication.
- **Built-in security** — encryption and authentication mechanisms included.

---

## Transport Layer

Provides end-to-end message transfer with error control, segmentation, flow control, and congestion control.

### TCP and Its IoT Variants

Standard TCP is connection-oriented and reliable but **heavyweight** for IoT.

| Variant                    | Benefit                                                                                                                                                                       |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **TCP-LP**                 | Lower latency                                                                                                                                                                 |
| **TCP Fast Open (TFO)**    | Embeds data in the initial SYN packet — reduces handshake round trips. Useful for devices that wake, send small data, then sleep. Also reduces exposure to SYN flood attacks. |
| **MPTCP (Multi-Path TCP)** | Uses multiple network interfaces simultaneously                                                                                                                               |

#### MPTCP Modes

- **Failover mode** — sends on one path (e.g. Wi-Fi); if it fails, switches to another (e.g. Zigbee). Improves reliability and availability.
- **Parallel mode** — splits the message across multiple interfaces simultaneously. Faster throughput.

> **Note:** MPTCP requires multiple network interfaces (Wi-Fi + cellular + Ethernet). Useful in gateways and smartphones, not small sensors.

**MPTCP Security Benefits:**

- Traffic splitting makes eavesdropping harder (attacker must intercept all paths)
- If one network is blocked/compromised, others continue
- Harder traffic analysis across split paths

---

## Application Layer

### MQTT (Message Queuing Telemetry Transport)

Lightweight **publish-subscribe** protocol. Designed for bandwidth-limited, power-constrained devices.

- Device **publishes** data to a **broker** on a specific topic
- **Subscribers** receive only the topics they subscribed to
- Broker filters — subscribers only get relevant data

**Why MQTT over HTTP/TCP?**

|             | HTTP                | TCP Socket        | MQTT               |
| ----------- | ------------------- | ----------------- | ------------------ |
| Model       | Request-reply       | Direct connection | Pub-Sub via broker |
| Overhead    | High (text headers) | Medium            | Very low (binary)  |
| Scalability | Poor                | Poor              | High               |
| Power       | High                | High              | Low                |

**MQTT power savings:** event-driven (no polling), sleep mode support, smart broker filtering, configurable QoS.

---

### CoAP (Constrained Application Protocol)

Lightweight alternative to HTTP for constrained devices.

- Uses **UDP** instead of TCP → less overhead
- **Binary** message format → smaller packets
- Supports **asynchronous** communication via CoAP-observe (device subscribes to updates, server pushes on change)
- Secured with **DTLS**

**HTTP vs CoAP:**

|           | HTTP            | CoAP            |
| --------- | --------------- | --------------- |
| Transport | TCP             | UDP             |
| Format    | Text (JSON/XML) | Binary          |
| Overhead  | High            | Low             |
| Security  | TLS             | DTLS            |
| Target    | Web             | Constrained IoT |

---

## TLS vs DTLS

**TLS** (Transport Layer Security) — works over TCP, standard secure web communication.

How TLS works (simplified):

1. Client sends supported encryption methods
2. Server responds with its public key + certificate
3. Client verifies certificate
4. Client encrypts a session key using server's public key
5. Both sides now share the session key → encrypted communication begins

**DTLS** (Datagram TLS) — TLS adapted for UDP (used by CoAP).

|            | TLS                | DTLS           |
| ---------- | ------------------ | -------------- |
| Transport  | TCP                | UDP            |
| Latency    | Higher             | Lower          |
| Overhead   | Higher             | Lower          |
| Connection | Persistent         | Connectionless |
| Use in IoT | HTTP-based devices | CoAP devices   |

> Both TLS and DTLS now use **ECDHE** (Elliptic Curve Diffie-Hellman Ephemeral) for key exchange — faster and less computation than RSA.
