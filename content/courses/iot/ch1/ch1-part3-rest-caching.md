---
title: "Chapter 1, Part 3 – REST Caching"
date: 2026-05-31
description: "How REST APIs use caching to reduce latency and server load — client-side vs server-side caching explained."
tags: [iot, api, rest, caching, chapter1]
toc: true
weight: 3
---

## What Is API Caching?

> Caching in the REST communication model means storing a copy of a response at
> some intermediate point — client or server side — so that repeated requests for
> the same resource do not require a full round trip to the origin server.

Caching is one of REST's core performance advantages. It reduces latency, lowers
server load, and makes applications more responsive under repeated or concurrent
requests.

---

## Client-Side Caching

> Client-side caching occurs on the user's device — a web browser, mobile app, or
> IoT gateway — and is controlled by HTTP caching headers.

How it works:

1. The server includes caching headers (`Cache-Control`, `Expires`) in its HTTP
   response, specifying how long the response should be considered fresh.
2. The client stores the response locally.
3. When the same resource is needed again, the client checks its local cache first.
4. If the cached copy is still valid, it is reused — no new request is sent.

**Examples:** Web browsers caching HTML pages, CSS files, JavaScript, and images to
speed up page load times and reduce bandwidth usage.

---

## Server-Side Caching

> Server-side caching occurs on the server itself or on intermediary proxy servers,
> and is controlled by the server through HTTP response headers.

How it works:

1. The server sets `Cache-Control` and `Expires` headers in its responses.
2. Clients and intermediary proxies read these headers and cache the responses
   accordingly.
3. Subsequent requests for the same resource can be served from the proxy cache
   without reaching the origin server.

**Examples:** Servers caching the results of database queries, computed API
responses, or dynamic data that is expensive to regenerate on every request.

---

## Client-Side vs Server-Side Caching

| Aspect | Client-Side | Server-Side |
|---|---|---|
| **Location** | User's device (browser, app) | Origin server or proxy |
| **Control** | Client, guided by server headers | Server |
| **Purpose** | Reduce repeated fetches for the same resource | Reduce redundant server processing |
| **Headers used** | `Cache-Control`, `Expires` | `Cache-Control`, `Expires` (set by server) |
| **Examples** | Browser cache (HTML, CSS, JS, images) | Cached DB queries, computed API responses |

---

## Why Caching Matters in IoT

IoT deployments often involve many devices polling the same endpoints repeatedly —
sensor status checks, configuration reads, firmware version queries. Without
caching:

- The server receives hundreds or thousands of identical requests per minute.
- Response time degrades under load.
- Bandwidth and processing resources are wasted on redundant work.

With caching, a single computed response is served to many clients without
repeating the underlying work.

---

