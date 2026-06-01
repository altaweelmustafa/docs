---
title: "Chapter 1, Part 5 – WebSocket APIs & Exclusive Pair"
date: 2026-05-31
description: "How WebSocket APIs enable real-time bidirectional IoT communication, the Exclusive Pair model, and a comparison with REST."
tags: [iot, api, websocket, real-time, chapter1]
toc: true
weight: 5
---

## What Are WebSocket APIs?

> WebSocket APIs enable bi-directional, full-duplex communication between a client
> and a server over a single persistent connection.

Unlike REST, where the client always initiates a request and waits for a response,
WebSocket allows both sides to send messages to each other at any time after the
connection is established.

WebSocket APIs follow the **Exclusive Pair** communication model.

---

## Why WebSocket for IoT?

WebSocket is the standard choice for IoT scenarios requiring real-time data
exchange:

- **Remote monitoring and control systems** — commands and readings flow
  continuously in both directions.
- **Streaming sensor data** — a sensor pushes updates as they occur rather than
  waiting to be polled.
- **Real-time alerts** — the server pushes a notification the moment a threshold is
  crossed.
- **Live device commands** — an operator sends a command; the device executes it
  immediately.

**Example:** A fleet management system uses WebSocket to continuously stream GPS
coordinates from vehicles to a central server, enabling live location tracking and
monitoring.

---

## How WebSocket Reduces TCP Overhead

### Persistent Connection

In standard HTTP (used by REST), a new TCP connection is established for every
request-response cycle. WebSocket instead establishes one persistent, full-duplex
connection that stays open. Both sides can send data at any time without the cost
of re-establishing the connection.

### Reduced Handshake Overhead

WebSocket begins with a one-time HTTP handshake to upgrade the connection. After
that, all data exchanges happen directly over the open socket — no repeated
handshakes, no per-message overhead. This significantly reduces latency for
high-frequency communication.

---

## WebSocket Connection Lifecycle

```
Client ──► Request to setup WebSocket Connection ──► Server
Client ◄── Response accepting the request         ◄── Server
              [Initial Handshake over HTTP]

Client ◄──► Data frame                            ◄──► Server
Client ◄──► Data frame                            ◄──► Server
              [Bidirectional over persistent WebSocket connection]

Client ──► Connection close request               ──► Server
Client ◄── Connection close response              ◄── Server
```

---

## WebSocket IoT Scenario: Sensor Network

1. Each sensor establishes a WebSocket connection with the central server.
2. Once connected, sensors push data updates in real-time — no repeated HTTP
   requests needed.
3. The central server processes incoming sensor data and may push alerts or
   dashboard updates to clients (web app, mobile app) over their own WebSocket
   connections.
4. Result: bidirectional, dynamic communication between multiple devices and a
   central server simultaneously.

---

## The Exclusive Pair Communication Model

> Exclusive Pair is a bidirectional, fully duplex communication model that uses a
> persistent connection between client and server. The connection remains open
> until the client explicitly requests to close it.

Both client and server can send messages freely after the connection setup — no
polling, no waiting for the other side to initiate.

### Lifecycle

```
Client ──► Request to setup Connection    ──► Server
Client ◄── Response accepting the request ◄── Server

Client ──► Message from Client to Server  ──► Server
Client ◄── Message from Server to Client  ◄── Server

Client ──► Connection close request       ──► Server
Client ◄── Connection close response      ◄── Server
```

### IoT Example: Smart Thermostat + Air Conditioner

The thermostat and air conditioner establish an exclusive pair channel:

- The thermostat continuously sends temperature readings to the air conditioner.
- The air conditioner adjusts its settings in response.
- The air conditioner sends back status updates and energy consumption data.

This is tightly coupled, device-to-device coordination — each device knows the
other's state at all times without any intermediary polling.

---

## REST vs WebSocket

| Feature | REST | WebSocket |
|---|---|---|
| Communication model | Request-Response | Exclusive Pair (persistent) |
| Direction | Client initiates only | Fully bidirectional |
| Connection | New per request | Persistent, one-time handshake |
| Latency | Higher (new connection overhead) | Lower (open connection) |
| Best for | Periodic data retrieval, configuration | Real-time streaming, live control |
| IoT example | Temperature sensor POST every minute | Fleet GPS live tracking |

---

