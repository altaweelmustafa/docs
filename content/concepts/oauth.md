---
title: "OAuth"
date: 2026-05-31
_build:
  list: never
  render: always
---

**OAuth** (Open Authorization) is an authorization framework that lets one application
access resources on behalf of a user — without the user giving that application their
password.

---

**The core idea:**

Instead of sharing credentials, OAuth issues a limited-access **token**. The token
says *"this app is allowed to do X on behalf of this user"* — nothing more.

---

**Real example:**

You open an app and click *"Sign in with Google"*. You never give the app your Google
password. Instead:

1. The app redirects you to Google.
2. Google asks: *"Do you allow this app to access your profile?"*
3. You approve.
4. Google gives the app a **token**.
5. The app uses that token to access only what you approved.

---

**Key terms:**

- **Resource Owner** — you, the user.
- **Client** — the app requesting access.
- **Authorization Server** — the server that issues tokens (e.g. Google).
- **Resource Server** — the server holding the protected data.
- **Token** — a temporary key granting limited access.

---

**Why it matters in IoT:**

IoT platforms use OAuth to let third-party apps (dashboards, automation tools) access
device data without storing user credentials on every connected service.
