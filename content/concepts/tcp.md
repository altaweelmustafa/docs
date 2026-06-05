---
title: "TCP"
date: 2026-05-31
_build:
  list: never
  render: always
---

**TCP** (Transmission Control Protocol) is a connection-oriented transport protocol
that guarantees reliable, ordered, and error-checked delivery of data between two
devices.

---

**What makes TCP reliable:**

- **Connection establishment** — before any data is sent, TCP performs a
  **three-way handshake**: SYN → SYN-ACK → ACK. Both sides confirm they are ready.
- **Acknowledgements** — every packet received must be acknowledged. If no ACK
  arrives, the sender retransmits.
- **Ordering** — packets are numbered. If they arrive out of order, TCP reassembles
  them correctly before passing data to the application.
- **Error checking** — checksums detect corrupted packets; corrupted packets are
  discarded and retransmitted.
- **Flow control** — the receiver tells the sender how much data it can handle,
  preventing overflow.

---

**The cost:**

Reliability comes with overhead. Handshakes, acknowledgements, and retransmissions
add latency and consume more bandwidth than [UDP](/docs/concepts/udp).

---

**When to use TCP:**

Use TCP when correctness matters more than speed — file transfers, web pages, API
calls, email, database queries.

---

**Why it matters in IoT:**

IoT applications using [REST](/docs/concepts/rest) or [HTTPS](/docs/concepts/https)
run over TCP. When a command must be delivered reliably (turn off a valve, unlock a
door), TCP is the right choice.
