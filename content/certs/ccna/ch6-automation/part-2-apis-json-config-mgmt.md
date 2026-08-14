---
title: "Ch6, Part 2 - REST APIs, JSON, Config Management"
date: 2026-08-14
description: "HTTP methods, JSON syntax, Puppet vs Chef vs Ansible vs SaltStack comparison."
tags: [ccna, rest-api, json, ansible, puppet, chef, chapter6]
toc: true
weight: 2
---

## REST API basics

- Stateless: each request contains everything needed, server holds no client session state between calls.
- Uses standard HTTP methods:

| Method | Action |
|---|---|
| GET | read |
| POST | create |
| PUT | replace/update |
| PATCH | partial update |
| DELETE | remove |

- Responses commonly formatted as JSON.
- HTTP status codes: 2xx success, 3xx redirect, 4xx client error (e.g. 404 not found, 401 unauthorized), 5xx server error.

## JSON syntax essentials

- Data is key-value pairs: `"key": "value"`.
- Objects wrapped in `{}`, arrays wrapped in `[]`.
- Data types: string, number, boolean, null, object, array.

```json
{
  "hostname": "R1",
  "interfaces": ["Gi0/0", "Gi0/1"],
  "enabled": true
}
```

Be able to read a small JSON blob and pull out a specific value, that's the practical exam skill (not writing JSON from scratch).

## Configuration management tools

| Tool | Agent? | Push or pull | Language | Owner style |
|---|---|---|---|---|
| Puppet | Agent-based | Pull | Ruby DSL (manifests) | Ruby-based, declarative |
| Chef | Agent-based | Pull | Ruby (recipes/cookbooks) | Ruby-based, procedural-ish |
| Ansible | Agentless (uses SSH) | Push | YAML (playbooks) | simplest to start with |
| SaltStack | Can be either | Push (typically) | YAML | fast, uses a message bus |

Exam angle: Ansible is agentless and uses SSH, this is the fact most likely to be tested directly, since it's the differentiator from Puppet/Chef.
