---
title: "Chapter 1, Part 3 – Dependability, Security, and Scalability"
date: 2026-07-03
description: "Dependability requirements, faults/errors/failures, security mechanisms, and scalability formulas."
tags: [distributed-systems, dependability, security, scalability, chapter1]
toc: true
weight: 3
---

## Dependability

A distributed system is dependable when users can trust it to provide correct service when needed.

A component may depend on other components. This matters because failure can propagate: if one service depends on another failing service, the first service may also fail.

### Dependability requirements

| Requirement | Meaning | Simple example |
|---|---|---|
| Availability | The system is ready for use. | Website responds now. |
| Reliability | Continuous correct service. | Website works correctly for a long period. |
| Safety | Very low probability of catastrophic failure. | Medical/traffic system avoids dangerous output. |
| Maintainability | Failed system can be repaired easily. | Restart or replace component quickly. |

### Availability vs reliability

- **Availability** is about being usable at a specific moment.
- **Reliability** is about continuous correct behavior during an interval.

A system can have high availability but lower reliability if it recovers quickly but fails often.

---

## Reliability metrics

| Metric | Meaning |
|---|---|
| MTTF | Mean Time To Failure: average time until a component fails. |
| MTTR | Mean Time To Repair: average repair time. |
| MTBF | Mean Time Between Failures: `MTTF + MTTR`. |

Exam note:

- Increase `MTTF` to make failures less frequent.
- Decrease `MTTR` to recover faster.
- Both help dependability, but they are not the same.

---

## Failure, error, fault

| Term | Meaning | Example |
|---|---|---|
| Failure | Component does not meet specification. | Program crashes. |
| Error | Incorrect internal state that may lead to failure. | Wrong variable value. |
| Fault | Cause of the error. | Programming bug, hardware defect, bad design. |

Relationship:

```text
fault  →  error  →  failure
cause     bad state  visible wrong behavior
```

---

## Handling faults

| Method | Meaning | Example idea |
|---|---|---|
| Fault prevention | Prevent faults from occurring. | Better design, careful hiring, code standards. |
| Fault tolerance | Continue working despite faults. | Replication, backup server, retry. |
| Fault removal | Reduce or eliminate existing faults. | Testing, debugging, patching. |
| Fault forecasting | Estimate present/future faults and their effects. | Reliability analysis. |

---

## Security is part of dependability

A distributed system that is not secure is not dependable.

Main security requirements:

| Requirement | Meaning |
|---|---|
| Confidentiality | Information is disclosed only to authorized parties. |
| Integrity | Alterations can be made only in authorized ways. |
| Availability | Authorized users can access the system. |
| Authentication | Verify identity of users/services. |
| Authorization | Decide what verified users may do. |
| Non-repudiation | A party cannot deny an action later. |

---

## Security mechanisms

The slides introduce security mechanisms using keys and hashing.

### Encryption notation

`K(data)` means data is encrypted or decrypted using key `K`.

### Symmetric cryptosystem

In symmetric encryption, the same secret key is used for encryption and decryption.

```text
DK(EK(data)) = data
and DK = EK
```

Main issue: both parties need the same secret key, so key distribution is difficult.

### Asymmetric cryptosystem

In asymmetric encryption, one key is public and the other is private.

Common use:

- encrypt with public key, decrypt with private key,
- sign with private key, verify with public key.

Main issue: usually slower than symmetric encryption.

### Secure hashing

A secure hash function `H(data)` returns a fixed-length string.

Important properties:

- small data change gives completely different hash,
- hard to find two inputs with same hash,
- useful for integrity checking.

---

## Scalability

A scalable system can handle growth without unacceptable performance loss or management complexity.

### Three types of scalability

| Type | Growth dimension | Example problem |
|---|---|---|
| Size scalability | More users, processes, or data. | One server cannot handle all requests. |
| Geographical scalability | Larger physical distance. | WAN latency makes synchronous calls slow. |
| Administrative scalability | More organizations/domains. | Different policies, security rules, payment rules. |

---

## Size scalability bottlenecks

Centralized solutions often hit limits in:

1. CPU computation capacity.
2. Storage capacity and disk transfer rate.
3. Network capacity between users and the service.

---

## Formal queue model for centralized service

Assume a centralized service with:

- request arrival rate `λ`,
- processing capacity `μ` requests/second,
- service time `S = 1/μ`.

### Utilization

Utilization is the fraction of time the service is busy:

```text
U = λ / μ
```

### Probability of k requests

```text
pk = (1 - U) U^k
```

### Average number of requests

```text
N = U / (1 - U)
```

### Throughput

```text
X = λ
```

### Response time

Response time is total time after a request is submitted:

```text
R = N / X = S / (1 - U)
R / S = 1 / (1 - U)
```

Important conclusion:

- If `U` is small, response time is close to service time.
- If `U` approaches 1, response time becomes extremely large.
- Solution: reduce service time `S`, add capacity, partition work, cache, or replicate.

---

## Geographical scalability problems

Going from LAN to WAN is not simple.

Problems:

- Synchronous client-server interactions become slow due to latency.
- WAN links are less reliable than LAN links.
- Streaming or interactive systems may fail if latency/bandwidth assumptions are wrong.

---

## Administrative scalability problems

Administrative scalability fails when different organizations have conflicting policies.

Examples:

- who pays for resource usage,
- who manages the service,
- what security rules apply,
- who can access which resources.

---

## Techniques for scaling

| Technique | Main idea | Example |
|---|---|---|
| Hide communication latency | Use asynchronous communication. | Send request and handle reply later. |
| Move computation to client | Let clients do work. | Browser scripts, Java applets historically. |
| Partition data/computation | Split work across machines. | DNS, WWW, sharded databases. |
| Replication | Store multiple copies. | Replicated file servers, replicated databases. |
| Caching | Keep temporary copies close to user. | Browser cache, proxy cache, CDN. |

---

## The replication problem

Replication helps performance and availability, but creates consistency problems.

If there are multiple copies and one copy is modified, that copy becomes different from the others.

Keeping all copies always consistent usually requires global synchronization, which hurts scalability.

Exam sentence:

> Replication improves availability and performance, but consistency is the price.

---

## Exam check

1. Explain failure, error, and fault with one example.
2. What is the difference between availability and reliability?
3. Why does security belong to dependability?
4. Given `λ` and `μ`, compute utilization and response time.
5. Why does replication hurt consistency?
