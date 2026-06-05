---
title: "Exclusive Pair"
date: 2026-05-31
_build:
  list: never
  render: always
---

**Exclusive Pair** is a communication model where a **persistent, dedicated connection**
is established between exactly two specific endpoints — and both sides can send and
receive data freely as long as the connection is open.

---

**Key characteristics:**

- **Persistent** — the connection stays open until one side explicitly closes it.
  No reconnecting for each message.
- **Bidirectional** — both sides can send data at any time, independently.
- **Dedicated** — the connection is between a specific client and a specific server,
  not shared with others.

---

**How it differs from request-response:**

In a standard [REST](/docs/concepts/rest) interaction, the client sends a request and
the server replies — then the connection closes. For the next message, a new connection
must be opened.

In an exclusive pair model ([WebSocket](/docs/concepts/websocket)), the connection
opens once and stays open. Either side can push data at any moment without waiting
for the other to ask.

---

**Real example:**

A smart thermostat and a smart air conditioner establish an exclusive pair channel.
The thermostat continuously pushes temperature readings; the AC pushes back its
current energy consumption. Both devices react to each other in real time without
constantly reconnecting.

---

**Used in:** WebSocket APIs, device-to-device IoT coordination, real-time dashboards,
live sensor feeds.
