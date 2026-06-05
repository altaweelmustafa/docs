---
title: "IP"
date: 2026-05-31
_build:
  list: never
  render: always
---

**IP** (Internet Protocol) is the fundamental protocol that gives every device on a
network a unique address and defines how data is packaged and routed from one device
to another across networks.

---

**IP Address:**

An IP address is a unique numerical label assigned to every device on a network.
Think of it as a postal address — without it, nobody knows where to deliver data.

- **IPv4** — 32-bit address, written as four numbers separated by dots: `192.168.1.1`
  Supports ~4.3 billion unique addresses (running out).
- **IPv6** — 128-bit address, written in hexadecimal: `2001:db8::1`
  Supports an astronomically large number of addresses — designed for IoT scale.

---

**What IP does:**

- **Addressing** — assigns source and destination addresses to every packet.
- **Routing** — determines the path a packet takes across networks to reach its destination.
- **Fragmentation** — breaks large packets into smaller pieces if needed, reassembles at destination.

---

**What IP does NOT do:**

IP is a best-effort protocol — it does not guarantee delivery, order, or error
correction. Those responsibilities fall on higher-level protocols like
[TCP](/docs/concepts/tcp) or [UDP](/docs/concepts/udp).

---

**Why it matters in IoT:**

Every IoT device needs an IP address to communicate over a network. IPv6 is
especially important in IoT because IPv4 addresses are exhausted and IoT deployments
can involve millions of devices.
