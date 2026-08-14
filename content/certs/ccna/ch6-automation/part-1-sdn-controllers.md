---
title: "Ch6, Part 1 - SDN and Controller-Based Networking"
date: 2026-08-14
description: "Traditional vs controller-based architecture, northbound/southbound APIs, Cisco DNA Center, ACI."
tags: [ccna, sdn, controllers, chapter6]
toc: true
weight: 1
---

## Traditional vs controller-based

| Model | Control plane | Management |
|---|---|---|
| Traditional | distributed, each device runs its own (e.g. OSPF process) | device-by-device CLI |
| Controller-based (SDN) | centralized on a controller | one place configures the whole fabric |

Controller-based networking trades per-device autonomy for centralized visibility, consistency, and faster changes at scale.

## Northbound vs southbound

| API direction | Between | Example |
|---|---|---|
| Northbound | Controller to applications/orchestration | REST APIs, used by DNA Center apps |
| Southbound | Controller to network devices | OpenFlow, NETCONF, OpFlex, CLI/SNMP |

Mnemonic: north is "up" toward the apps/humans, south is "down" toward the hardware.

## Cisco DNA Center

Cisco's SDN controller for enterprise campus. Key ideas to know by name: automation (push config to many devices at once), assurance (analytics/telemetry), policy-based segmentation via SD-Access, and it exposes northbound REST APIs.

## Cisco ACI

Cisco's SDN for the data center, controlled by the APIC (Application Policy Infrastructure Controller). Key terms: spine-leaf underlay (physical fabric), overlay (VXLAN-based logical networks), endpoint groups (EPGs) instead of traditional VLAN-based policy.

## Underlay vs overlay

| Term | Meaning |
|---|---|
| Underlay | the physical network providing basic IP connectivity |
| Overlay | a virtual network built on top (e.g. VXLAN tunnels) carrying the actual logical topology |
