---
title: "Chapter 5: Block Cipher Operation — Multiple Encryption & Modes"
date: 2026-08-23
description: "Two-key vs. three-key Triple DES and the attacks against it, why block ciphers need modes of operation, and the five NIST modes: ECB, CBC, CFB, OFB, and CTR."
tags: [information-security, cryptography, block-ciphers, triple-des, modes-of-operation, chapter5]
toc: true
weight: 5
---

## Chapter 5 goal

A block cipher only defines how to transform one fixed-size block. This chapter covers the two remaining practical questions: how to get extra strength out of DES via multiple encryption (two-key vs. three-key 3DES, and the attacks against each), and how to actually apply a block cipher to real, multi-block messages via NIST's five standardized modes of operation.

Study order:

1. Multiple encryption, two-key and three-key Triple DES, and why block ciphers need modes of operation in the first place.
2. Electronic Codebook (ECB) and Cipher Block Chaining (CBC) modes.
3. The stream-oriented modes: Cipher Feedback (CFB) and Output Feedback (OFB).
4. Counter (CTR) mode, and comparing all five modes.
