---
title: "Chapter 3, Part 5 – Ad-Hoc Network Attacks"
description: "Wormhole, black-hole, selective forwarding, and sinkhole attacks in ad-hoc and IoT networks."
---

## Wormhole Attack

One of the most popular attacks in ad-hoc networks.

- The intruder **does not need to compromise any host** in the network.
- The attacker captures packets at one location, tunnels them to another node (via a private "wormhole" link), and retransmits them from there.
- This makes the attacker appear as a close, high-quality neighbour to distant nodes — **distorting the network topology**.
- Wormhole attacks are **very strange and difficult to identify**.

### How to Detect a Wormhole Attack

- **Hop Count vs Physical Distance** — check whether the reported hop count matches the actual physical distance. A suspiciously short hop count for a long distance is a red flag.
- **Packet Travel Time (RTT)** — measure how long packets take to travel. Wormholes may show abnormally fast delivery.
- **Neighbour Verification** — check whether claimed neighbours are actually within normal radio range.

---

## Black-Hole Attack

- The malicious node **falsely advertises a good (short) path** to the destination during route discovery.
- It sends fake hop counts to attract traffic toward itself.
- Once selected as a relay, the malicious node simply **drops all received packets**.

Result: packets are silently lost, disrupting communication without any visible error.

---

## Selective Forwarding Attack

- Similar to black-hole, but instead of dropping everything, the malicious node **selectively drops certain packets** and forwards the rest.
- This makes detection much harder — the node appears to be functioning normally most of the time.
- Dropped packets may carry **sensitive or critical data** needed for further processing.

---

## Sinkhole Attack

- Sensors left unattended in a network for long periods are especially vulnerable.
- The **compromised node attracts traffic from all surrounding nodes** by advertising itself as the best route.
- Once it controls the flow of data, the attacker can launch further attacks such as:
  - Selective forwarding
  - Fabrication
  - Modification
