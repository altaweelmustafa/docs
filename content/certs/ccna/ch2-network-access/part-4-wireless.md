---
title: "Ch2, Part 4 - Wireless Fundamentals"
date: 2026-08-14
description: "AP modes, WLC/CAPWAP, SSID/BSSID, 802.11 standards, channels."
tags: [ccna, wireless, wlc, chapter2]
toc: true
weight: 4
---

## AP architecture

| Mode | Description |
|---|---|
| Autonomous AP | standalone, config on the AP itself, no controller |
| Lightweight AP | config pushed from a WLC, uses CAPWAP tunnel to the controller |

- CAPWAP: control plane over UDP 5246, data plane over UDP 5247.
- WLC handles: RF management, roaming, security policy, centralized config for all lightweight APs.

## SSID / BSSID / ESSID

| Term | Meaning |
|---|---|
| SSID | the network name you see and connect to |
| BSSID | MAC address of one specific AP radio |
| ESSID | same SSID broadcast from multiple APs, forms one extended network for roaming |

## 802.11 standards

| Standard | Band | Max speed (theoretical) |
|---|---|---|
| 802.11a | 5 GHz | 54 Mbps |
| 802.11b | 2.4 GHz | 11 Mbps |
| 802.11g | 2.4 GHz | 54 Mbps |
| 802.11n (Wi-Fi 4) | 2.4/5 GHz | 600 Mbps |
| 802.11ac (Wi-Fi 5) | 5 GHz | ~1.3 Gbps+ |
| 802.11ax (Wi-Fi 6) | 2.4/5/6 GHz | much higher, better efficiency in dense environments |

## Channels

- 2.4 GHz: only channels 1, 6, 11 are non-overlapping in most regions. Everything else overlaps.
- 5 GHz: many more non-overlapping channels, less interference, shorter range than 2.4 GHz.

## Client authentication (preview, full detail in Ch5)

- Personal (PSK): shared passphrase.
- Enterprise (802.1X): per-user auth against a RADIUS server.
- Guest: open or web-auth portal, usually isolated VLAN.
