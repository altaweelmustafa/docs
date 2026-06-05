---
title: "HTTPS"
date: 2026-05-31
_build:
  list: never
  render: always
---

**HTTPS** (HyperText Transfer Protocol Secure) is the secure version of HTTP — the
protocol used to transfer data between a browser (or any client) and a web server.

The "secure" part comes from **TLS** (Transport Layer Security), which wraps the HTTP
communication in an encrypted tunnel. This means:

- Data sent between client and server is **encrypted** — nobody intercepting the
  traffic can read it.
- The server's identity is **verified** via a digital certificate — you know you're
  talking to the real server, not an impersonator.
- Data **integrity** is guaranteed — it cannot be modified in transit without detection.

---

**How it works (simplified):**

1. Client connects to server and says *"I want to talk securely."*
2. Server sends its **TLS certificate** (issued by a trusted Certificate Authority).
3. Client verifies the certificate is legitimate.
4. Both sides agree on an **encryption key** (handshake).
5. All further communication is encrypted using that key.

---

**Why it matters in IoT:**

IoT devices sending sensor data or receiving commands over the internet must use HTTPS
to prevent attackers from reading or injecting data into the communication.
