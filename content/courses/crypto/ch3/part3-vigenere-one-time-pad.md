---
title: "Chapter 3, Part 3 – Polyalphabetic Ciphers & the One-Time Pad"
date: 2026-08-09
description: "The Vigenère cipher, the Vernam cipher, and why the one-time pad is the only theoretically unbreakable cipher — plus why it's rarely practical."
tags: [information-security, cryptography, vigenere-cipher, one-time-pad, chapter3]
toc: true
weight: 3
---

## Polyalphabetic substitution ciphers

Instead of one fixed substitution rule (monoalphabetic), use **different** monoalphabetic substitutions as you move through the message. Common features:

1. A **set of related** monoalphabetic substitution rules.
2. A **key** determines which rule applies at each position.

This smooths out single-letter frequency statistics, since the same plaintext letter can map to different ciphertext letters depending on position.

---

## Vigenère cipher

The best-known, simplest polyalphabetic cipher. The "rule set" is the 26 Caesar shifts (0–25); each is identified by a **key letter** (the ciphertext letter that `a` maps to under that shift).

### Encryption

The key is a repeating keyword, written under the plaintext letter-by-letter:

```
key:        deceptivedeceptivedeceptive
plaintext:  wearediscoveredsaveyourself
ciphertext: ZICVTWQNGRZGVTWAVZHCQYGLMGJ
```

Each plaintext letter is Caesar-shifted by the amount indicated by the corresponding key letter.

**Strength:** multiple ciphertext letters can represent the same plaintext letter (one per unique key letter), obscuring single-letter frequency. **Weakness:** if the keyword is short, it repeats — and repetition still leaks structure. A longer keyword frequency-distribution table (e.g. keyword length 9) shows an improvement over Playfair, but considerable frequency information still survives.

> Exam sentence: Vigenère's security scales with keyword length — the fundamental flaw is that any *repeating* key, no matter how long, still leaves statistical patterns for a cryptanalyst to exploit (this is what motivated the Vernam cipher and, ultimately, the one-time pad).

---

## Vernam cipher

Introduced by AT&T engineer Gilbert Vernam (1918). Works on **binary data** rather than letters. The key idea: use a keyword as long as the plaintext, generated from a running loop of tape — but because the tape *eventually repeats*, it's really still a very long repeating key. Given enough ciphertext (or known/probable plaintext), it can still be broken.

---

## The one-time pad

Proposed by Army Signal Corps officer Joseph Mauborgne as the ultimate fix to the Vernam cipher's weakness:

- Use a **truly random key**, as long as the message itself, so it never needs to repeat.
- The key is used for **exactly one message**, then discarded.
- Every new message gets a brand-new random key of matching length.

### Why it's unbreakable

The scheme produces ciphertext that is statistically indistinguishable from random noise — it bears **no relationship whatsoever** to the plaintext. In fact, for any ciphertext, there exists *some* key that decrypts it to *any* plaintext of the same length. An exhaustive key search would surface every possible legible plaintext with no way to know which one is correct.

This makes the one-time pad the **only cryptosystem with proven perfect secrecy** — its security rests entirely on the randomness of the key.

### Why it's rarely used in practice

Despite perfect theoretical security, two practical problems limit it:

1. **Generating truly random keys at scale** — a heavily used system might need millions of random characters; generating genuine randomness in that volume is hard.
2. **Key distribution** — a key as long as *every* message must reach both parties securely. This is a "mammoth" logistics problem, arguably harder than the original secure-communication problem.

Because of these, the one-time pad is mainly reserved for **low-bandwidth, very-high-security channels** (e.g. historically, diplomatic and intelligence communications).

> Exam sentence: perfect secrecy ≠ practical usability. The one-time pad is provably unbreakable but its key-management burden makes it impractical for general-purpose, high-volume communication.

---

## What to remember for the exam

Vigenère: multiple Caesar shifts driven by a repeating keyword — better than monoalphabetic, but a repeating key still leaks patterns. Vernam: binary version, still effectively a repeating key over time. One-time pad: truly random, message-length, single-use key → provably perfect secrecy, but impractical due to key generation and distribution costs.

---

## Exam check

1. Using keyword `"LEMON"`, encrypt the plaintext `"ATTACK"` with the Vigenère cipher.
2. Why does a repeating keyword still leave the cipher vulnerable, no matter how long it is?
3. Explain why the one-time pad is unbreakable, and name its two practical limitations.
