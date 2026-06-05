---
title: "JWT"
date: 2026-05-31
_build:
  list: never
  render: always
---

**JWT** (JSON Web Token) is a compact, self-contained token used to securely transmit
information between two parties. It is commonly used for **authentication** and
**authorization** in web and IoT applications.

---

**Structure:**

A JWT is a string of three Base64-encoded parts separated by dots:

```
header.payload.signature
```

- **Header** — token type and signing algorithm (e.g. `HS256`).
- **Payload** — the actual data (claims): user ID, role, expiry time, etc.
- **Signature** — a cryptographic signature that proves the token hasn't been tampered with.

---

**How it works:**

1. User logs in with credentials.
2. Server verifies credentials and issues a JWT.
3. Client stores the JWT (in memory or a cookie).
4. On every subsequent request, the client sends the JWT in the request header.
5. Server verifies the signature — if valid, it trusts the payload without hitting the database.

---

**Key property — stateless:**

The server does not need to store session data. Everything needed to verify the
request is inside the token itself. This makes JWT ideal for distributed systems and
IoT where many devices communicate with many servers.

---

**OAuth vs JWT:**

OAuth is a *framework* defining how authorization flows work. JWT is a *token format*
often used to carry the access tokens OAuth issues. They are complementary, not
alternatives.
