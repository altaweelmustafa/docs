---
title: "Chapter 1, Part 1 – IoT Definition, Characteristics & Structure"
description: "IoT definition, key characteristics, how it works, the DIKW model, IoT structure, and the four main layers."
---

## Definition

**IoT** = physical objects embedded with sensors, processing ability, and software that connect and exchange data over the Internet — without human intervention.

"Things" include: heart monitoring implants, RFID tags on animals, environmental sensors, vehicles, and more.

---

## How IoT Works

IoT is not one single technology — it's a combination of:

- **Sensing** — collecting data from the environment
- **Actuation** — acting on the environment based on decisions
- **Communication & Cooperation** — devices talking to each other
- **Embedded processing** — local decision-making on the device
- **Addressability & Identification** — every device has a unique ID (IP, URI)
- **Localization** — knowing where a device is

---

## DIKW Model (Knowledge Management)

Data alone is not enough. It must be processed through:

```
Data → Information → Knowledge → Wisdom
```

AI + IoT use this hierarchy to make sense of massive data streams and generate meaningful decisions (used in cybersecurity, healthcare, business).

---

## Key Characteristics of IoT

- **Dynamic & Self-Adapting** — devices adapt based on context (e.g. a camera switching between day/night mode).
- **Self-Configuring** — large numbers of devices can work together automatically.
- **Interoperable** — devices from different manufacturers can communicate.
- **Unique Identity** — every device has a unique ID (IP address, URI).
- **Integrated into Information Networks** — devices share data with other systems (e.g. smart home sensors working together).

---

## Structure of IoT

IoT can be thought of as:

> "An anytime, anywhere, anything network of internet-connected physical devices capable of sensing and intelligently affecting their environment."

Key structural features:

- Massive number of constrained devices (low power, low memory)
- Intermittent and often unstable connectivity
- Uses RFIDs, wireless connections as enablers

**Four structural concepts:**

- **Tagging** — real-time traceability via RFID
- **Feeling** — sensors collecting environmental data
- **Shrinking** — miniaturization enabling tiny smart devices
- **Thinking** — embedded intelligence allowing simple automatic decisions

---

## IoT Main Layers

| Layer                 | Role                                            | Example Protocols                 |
| --------------------- | ----------------------------------------------- | --------------------------------- |
| **Perception Layer**  | Hardware — sensors & devices that collect data  | Zigbee, BLE, LoRaWAN, MQTT-SN     |
| **Network Layer**     | Transfers data between devices, gateways, cloud | IPv6, 6LoWPAN, RPL, IPSec         |
| **Support Layer**     | Cloud computing & processing resources          | TLS, MQTT, CoAP                   |
| **Application Layer** | User-facing services and control interfaces     | HTTP, WebSocket, REST, MQTT, XMPP |

---

## Logical Design Blocks

An IoT system is made up of these functional blocks:

- **Device Block** — IoT sensors + gateway devices
- **Communication Block** — handles data transmission, encoding, flow control
- **Services Block** — device monitoring, control, data publishing, discovery
- **Management Block** — firmware updates, provisioning, maintenance
- **Security Block** — authorization, integrity, encryption, access control
- **Application Block** — user interface for monitoring and control
