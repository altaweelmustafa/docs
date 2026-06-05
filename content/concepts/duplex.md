---
title: "Half Duplex / Full Duplex"
date: 2026-05-31
_build:
  list: never
  render: always
---

**Duplex** refers to how two devices communicate — specifically, whether they can
send and receive data at the same time.

---

## Half Duplex

Both sides can send and receive, but **not simultaneously**. Only one side transmits
at a time — the other must wait.

**Analogy:** A walkie-talkie. You press the button to talk, release to listen. Both
people cannot speak at the same time — one talks, the other waits.

**Characteristics:**
- Simpler and cheaper to implement.
- Lower bandwidth utilization — the channel is idle while one side waits.
- Collision risk if both sides try to transmit at the same time.

**Examples:** Walkie-talkies, older Ethernet (half-duplex mode), some IoT sensor
protocols.

---

## Full Duplex

Both sides can send and receive **simultaneously** — at the same time, over the same
connection.

**Analogy:** A phone call. Both people can speak and hear at the same time.

**Characteristics:**
- Higher throughput — no waiting.
- Requires either two separate channels or a protocol that separates the two
  directions on one channel.
- More complex but significantly more efficient.

**Examples:** Modern Ethernet, telephone networks, [TCP](/docs/concepts/tcp),
[WebSocket](/docs/concepts/exclusive-pair).

---

## Half vs Full — Quick Comparison

| | Half Duplex | Full Duplex |
|---|---|---|
| Simultaneous TX/RX | No | Yes |
| Complexity | Lower | Higher |
| Throughput | Lower | Higher |
| Collision risk | Yes | No |
| Example | Walkie-talkie | Phone call |

---

**Why it matters in IoT:**

Most modern IoT communication protocols aim for full duplex to enable real-time
bidirectional data flow — sensors pushing data while simultaneously receiving commands.
[WebSocket](/docs/concepts/exclusive-pair) and [TCP](/docs/concepts/tcp) are both
full-duplex protocols.
