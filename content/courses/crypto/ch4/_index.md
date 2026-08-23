---
title: "Chapter 4: Symmetric Block Ciphers — Feistel Structure, DES & Cryptanalysis"
date: 2026-08-23
description: "The Feistel cipher structure, the Data Encryption Standard, Simplified DES as a hand-computable teaching example, Double/Triple DES, and the structural cryptanalysis techniques (differential and linear) used to attack block ciphers."
tags: [information-security, cryptography, block-ciphers, des, feistel, cryptanalysis, chapter4]
toc: true
weight: 4
---

## Chapter 4 goal

Chapter 3 covered classical ciphers by hand. This chapter moves to modern symmetric block ciphers: the Feistel structure nearly all of them share, the most important historical example (DES), a small-scale version you can trace by hand (S-DES), why single DES was extended to Triple DES, and the two major structural attacks — differential and linear cryptanalysis — that shaped how block ciphers are designed today.

Study order:

1. Block ciphers and the Feistel cipher structure: substitution/permutation, diffusion/confusion, design parameters.
2. The Data Encryption Standard (DES): structure, strength, and the role of the number of rounds.
3. Simplified DES (S-DES): a full worked example you can compute by hand.
4. Double DES and the meet-in-the-middle attack, Triple DES, and the two major structural attacks on block ciphers — differential and linear cryptanalysis.
