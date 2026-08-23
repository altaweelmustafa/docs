---
title: "Chapter 6: Random Bit Generation & Stream Ciphers"
date: 2026-08-23
description: "True vs. pseudorandom number generators, the randomness/unpredictability requirements and testing that cryptographic PRNGs must meet, common PRNG algorithms (LCG, BBS, block-cipher-based), and stream ciphers (RC4, feedback shift registers)."
tags: [information-security, cryptography, random-numbers, prng, stream-ciphers, rc4, chapter6]
toc: true
weight: 6
---

## Chapter 6 goal

Random and pseudorandom bit streams underpin key generation, nonces, and stream ciphers throughout cryptography. This chapter covers what "random enough" means for cryptographic purposes, how PRNGs are built and validated, and how a PRNG becomes a practical stream cipher — with RC4 as the running example.

Study order:

1. True vs. pseudorandom number generators, and the randomness/unpredictability requirements a cryptographic bit stream must satisfy.
2. PRNG requirements, statistical testing, and seed generation.
3. PRNG algorithm design: the linear congruential generator, Blum Blum Shub, and block-cipher-based PRNGs.
4. Stream ciphers: general structure, design considerations, RC4, and feedback-shift-register-based ciphers.
