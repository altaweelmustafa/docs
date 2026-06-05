---
title: "UDP"
date: 2026-05-31
_build:
  list: never
  render: always
---

**UDP** (User Datagram Protocol) is a connectionless transport protocol that sends
data as fast as possible with no guarantees — no handshake, no acknowledgements, no
retransmissions.

---

**What UDP does NOT do:**

- No connection setup before sending.
- No confirmation that packets arrived.
- No retransmission of lost packets.
- No ordering of packets.

---

**Why would anyone use it?**

Because sometimes speed matters more than correctness. Dropping a packet is better
than waiting for a retransmission that arrives too late to be useful.

---

**When to use UDP:**

- **Live video/audio streaming** — a dropped frame is better than a delayed one.
- **Online gaming** — stale position data is useless; better to send the next update.
- **DNS lookups** — simple request-response, fast, retried at the application level if needed.
- **IoT sensor broadcasts** — periodic temperature readings where missing one value
  doesn't matter because the next one arrives in a second anyway.

---

**TCP vs UDP — quick comparison:**

| | TCP | UDP |
|---|---|---|
| Connection | Required (handshake) | None |
| Reliability | Guaranteed | Best-effort |
| Ordering | Yes | No |
| Speed | Slower | Faster |
| Overhead | Higher | Lower |
| Use case | Commands, files, APIs | Streams, broadcasts, real-time |

---

**Why it matters in IoT:**

Constrained IoT devices with limited battery and bandwidth often prefer UDP-based
protocols (like CoAP) over TCP-based ones to reduce overhead and extend network
lifetime.
