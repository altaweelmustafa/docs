---
title: "Chapter 1, Part 2 – REST APIs"
date: 2026-05-31
description: "How REST-based communication APIs work, their architectural principles, HTTP methods, and advantages in IoT systems."
tags: [iot, api, rest, http, chapter1]
toc: true
weight: 2
---

## What Is REST?

> REST (Representational State Transfer) is a set of architectural principles for
> designing web services that focus on a system's resources and how their states
> are addressed and transferred.

REST APIs follow the **request-response** communication model. An HTTP client sends
a request; the server processes it and sends back a response. The REST constraints
apply to all components, connectors, and data elements within a distributed
hypermedia system.

**IoT example:** A temperature sensor periodically sends readings to a cloud server
by making HTTP POST requests to a RESTful API endpoint.

---

## How REST Works

A REST-aware HTTP client communicates with a RESTful web service through an HTTP
packet. The packet contains:

- An **HTTP command** (GET, PUT, POST, DELETE)
- A **REST payload** in JSON or XML format

The server holds resources identified by URIs, each with one or more
representations.

---

## HTTP Methods in REST

| Method | Purpose |
|---|---|
| **GET** | Retrieve a resource |
| **POST** | Submit new data to the server |
| **PUT** | Update an existing resource |
| **DELETE** | Remove a resource |

---

## Advantages of REST

### Simplicity and Ease of Use

REST is built on standard HTTP methods that are already universally understood and
supported. Developers do not need to learn new protocols or tools — if you know
HTTP, you can use REST.

### Flexibility and Scalability

REST APIs are platform-independent. They work with any programming language or
system that supports HTTP, making them highly portable across different environments
and easy to scale.

### Statelessness

> Each request from a client contains all the information needed to process it.
> The server holds no client state between requests.

This simplifies server implementation and allows for better scalability — no session
storage or state synchronization required on the server side.

### Caching

REST can take advantage of HTTP caching mechanisms at multiple layers — client,
proxy, and server — to improve performance and reduce server load. Frequently
requested responses are stored and reused rather than recomputed.

### Security

REST can leverage existing HTTP security mechanisms:

- **HTTPS** for encrypted communication.
- **OAuth** authentication headers.
- **JWT (JSON Web Tokens)** for access control.

---

