---
title: "Chapter 1, Part 3 – Communication Models & APIs"
description: "IoT communication models (Request-Response, Pub-Sub, Push-Pull, Exclusive Pair), edge computing, and API types (REST, WebSocket, Open, Partner, Internal, Composite)."
---

## Communication Models

### 1. Request-Response

Client sends a request → server responds.

- Simple and widely used
- Used by: **REST APIs**, HTTP
- Example: mobile app requests current temperature → device responds with reading

---

### 2. Publish-Subscribe

Publishers send data to **topics** managed by a **broker**. Subscribers receive only topics they care about. Publishers and subscribers don't know each other.

- Reduces network traffic (data sent once, broker distributes)
- Used by: **MQTT**
- Example: temperature sensor publishes to `home/temp` → dashboard subscribes and receives updates

---

### 3. Push-Pull

Producers **push** data into a **queue**. Consumers **pull** from the queue when ready.

- Queue acts as a **buffer** — decouples producers and consumers
- Handles mismatch between production rate and consumption rate
- Buffer size matters: too small → producer waits; too large → more memory used
- Example: weather station pushes updates to a queue; clients pull historical data on demand

---

### 4. Exclusive Pair

Bidirectional, **full-duplex**, persistent connection between client and server.

- Connection stays open until client closes it
- Both sides can send messages freely after connection setup
- Used by: **WebSocket**
- Advantages: reduces congestion, predictable performance, enhanced security
- Disadvantages: limited scalability, single point of failure, higher complexity
- Example: smart thermostat and AC maintaining a persistent link to coordinate temperature

---

## Edge Computing

Processing data **closer to the device** rather than sending everything to the cloud.

**Advantages:**

- Lower latency — real-time responses
- Reduces bandwidth to cloud
- Better privacy — sensitive data stays local
- More reliable — works even if cloud connection drops
- Scalable — distributes load across edge nodes

**Limitations:**

- Limited processing power at edge
- Limited memory and storage
- Harder to manage (distributed system)
- Increased attack surface — more physical devices to secure
- Dependency on third-party edge providers

---

## APIs in IoT

An **API** is a set of protocols that defines how things in an IoT network communicate and interact with each other.

**What APIs allow:**

- **Device Control** — turn lights on/off, adjust thermostat
- **Data Retrieval** — read sensor values or device status
- **Event Handling** — subscribe to alerts (motion detected, door opened)
- **Configuration Management** — update schedules, thresholds, parameters

---

### API Types

| Type                   | Access                 | Description                                                                      |
| ---------------------- | ---------------------- | -------------------------------------------------------------------------------- |
| **Open / Public**      | Anyone                 | Free, publicly documented, no approval needed. Example: Samsung SmartThings API  |
| **Partner**            | Approved partners only | Requires license/agreement. Access controlled via API keys. Example: AWS IoT API |
| **Internal / Private** | Organization only      | Used internally, not exposed externally. Docs restricted to internal teams       |
| **Composite**          | Mixed                  | Combines multiple API types. Synchronous, aimed at speed and efficiency          |

---

### REST API

Follows **Request-Response** model. Uses standard HTTP methods (GET, POST, PUT, DELETE).

- **Stateless** — each request carries all needed info
- **Cacheable** — responses can be cached (client-side or server-side) to reduce load
- **Flexible** — works across any platform that supports HTTP
- **Secure** — leverages HTTPS, OAuth, JWT

> Example: temperature sensor periodically sends readings via HTTP POST to a RESTful cloud endpoint.

**Caching:** client checks local cache before making a new request. Server sets cache rules via `Cache-Control` and `Expires` headers. Reduces repeated requests and server load.

**Proxy server** sits between client and server — handles caching, content filtering, anonymity, and security inspection.

---

### WebSocket API

Follows **Exclusive Pair** model. Full-duplex, persistent connection.

- Single handshake → connection stays open
- No repeated TCP handshakes per message → low overhead
- Used for real-time applications: remote monitoring, live dashboards, streaming sensor data
- Example: fleet management system streams GPS data from vehicles to central server continuously
