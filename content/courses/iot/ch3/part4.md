---
title: "Chapter 3, Part 4 – Ad-Hoc Networks & AODV Routing"
description: "Ad-hoc wireless network fundamentals, why routing is challenging, and how AODV discovers and maintains routes using RREQ, RREP, and sequence numbers."
---

## Ad-Hoc Wireless Networks

- In ad-hoc networks, each node can communicate directly with other nodes — **no access point** is required for access control.
- Nodes themselves take care of **routing** (finding the best path between source and destination).
- Every node maintains a **routing table** containing information about other nodes.
- The network is **dynamic** — routing tables change constantly as nodes move or leave.
- Ad-hoc networks are **asymmetric** — the upload and download paths between two nodes may be different.

### Why Routing is Challenging

- Nodes move frequently
- Connections may break
- New nodes join the network
- Existing nodes leave
- Multiple routes may exist simultaneously

---

## AODV — Ad-Hoc On-Demand Distance Vector

AODV is a routing protocol for mobile/ad-hoc networks. It discovers routes **only when needed** (on-demand), rather than maintaining full routing tables for all nodes at all times.

Key properties:

- Routes are **loop-free** (guaranteed by sequence numbers)
- Finds the **shortest route** possible
- Handles **route changes** and creates new routes on error
- Nodes track neighbours by listening for **HELLO messages** broadcast at set intervals

---

## AODV Route Discovery

When node A wants to send to node B but B is not a direct neighbour, A broadcasts a **Route Request (RREQ)**:

```
RREQ contains:
  - Source node
  - Destination node
  - Lifespan (TTL)
  - Sequence Number (unique ID)
```

When a neighbour receives the RREQ, it has two choices:

- If it **knows a route** to the destination (or IS the destination) → send a **Route Reply (RREP)** back.
- If it **doesn't know** → rebroadcast the RREQ to its own neighbours.

The RREQ keeps propagating until its lifespan expires. If no reply is received in time, the source rebroadcasts with a **longer lifespan and new ID**.

Nodes use the **Sequence Number** to avoid rebroadcasting the same RREQ twice.

---

## Sequence Numbers

Sequence numbers act as **timestamps** — they indicate how "fresh" a route is.

- Every time a node sends any message, it **increments its own sequence number**.
- Each node records the sequence numbers of all nodes it communicates with.
- A **higher sequence number = fresher route**.
- When a node receives a RREP with a better sequence number than what it currently has, it **replaces its old route** with the new one.

---

## AODV Advantages

- **On-demand routing** — routes established only when needed, saving resources.
- **Low memory usage** — nodes store routes only to active destinations.
- **Loop-free routes** — sequence numbers ensure up-to-date, loop-free paths.
- **Adaptable to mobility** — suitable for dynamic topologies like MANETs and IoT.
- **Scalable** — works efficiently in environments with moderate node counts.

## AODV Limitations

- **High latency in route discovery** — initial delay when no route exists.
- **Broadcast overhead** — flooding RREQ messages consumes bandwidth in dense networks.
- **No multiple route support** — maintains only one route per destination.
- **Reactive route maintenance** — causes packet loss during route breaks.
- **Security vulnerabilities** — susceptible to blackhole, wormhole, and RREQ flooding attacks.
