---
title: "Ch6, Part 3 - Hands-On Drills"
date: 2026-08-14
description: "Practical drills for REST APIs, JSON, and push vs pull config management, no Packet Tracer needed."
tags: [ccna, drills, rest-api, json, ansible, chapter6]
toc: true
weight: 3
---

Packet Tracer doesn't cover this domain well. Use a terminal with `curl` (and `jq` if you have it) instead, against any free public test API (search for a free "JSON placeholder" test API to point `curl` at).

## Easy

**1. Read an HTTP response.** Send a GET request with `curl -i` to a free test API endpoint. Identify the HTTP status code in the response and state whether the request succeeded, and which method (GET/POST/PUT/DELETE) you used.

**2. Extract values from JSON by hand.** Write a small JSON object describing a router: `hostname`, an `interfaces` array with 2-3 entries, and an `enabled` boolean. Without any tool, by reading the text, write down the value of each field and the data type of each (string, array, boolean).

## Hard

**1. GET, POST, and extract a nested field.** Send a GET to a free test API and a POST with a small JSON body of your own. Capture both JSON responses. From the GET response, extract a field that's nested two levels deep (an object inside an object, or a value inside an array of objects) and write down the exact `jq`-style path (or manual description) you used to find it.

**2. Push vs pull, on paper.** Write a short YAML snippet that looks like an Ansible task setting a device's hostname (doesn't need to run, just needs correct YAML structure: task name, module, parameter). Next to it, write one sentence describing how Puppet would achieve the same result differently. Then explain in your own words which one needs an agent process running on the managed device and continuously checking in, and which one just needs SSH reachability, and why that maps to "pull" vs "push."
