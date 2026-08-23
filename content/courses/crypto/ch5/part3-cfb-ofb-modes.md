---
title: "Chapter 5, Part 3 – Cipher Feedback (CFB) & Output Feedback (OFB)"
date: 2026-08-23
description: "Turning a block cipher into a stream cipher: CFB's s-bit shift-register feedback of ciphertext, and OFB's full-block feedback of the encryption output, plus their error-propagation trade-off."
tags: [information-security, cryptography, modes-of-operation, cfb, ofb, chapter5]
toc: true
weight: 3
---

## Turning a block cipher into a stream cipher

A block cipher normally encrypts a full `b`-bit block at a time (`b = 64` for DES, `128` for AES). Three modes — CFB, OFB, and CTR (next part) — convert a block cipher into something that behaves like a stream cipher: no padding to a full block is required, and encryption can happen in real time as data arrives. Ideally, ciphertext length matches plaintext length exactly, so no transmission capacity is wasted.

---

## Cipher Feedback (CFB) mode

CFB processes plaintext in **`s`-bit segments** (a common choice is `s = 8`), chaining them together the way CBC chains full blocks — the ciphertext of any segment depends on all preceding plaintext.

**Encryption:** the encryption function's input is a `b`-bit shift register, initialized to an IV. The **leftmost `s` bits** of the encryption function's output are XORed with the first `s`-bit plaintext segment to produce the first ciphertext unit. The shift register then shifts left by `s` bits, with the new ciphertext unit placed in the rightmost `s` bits — and the process repeats.

**Decryption** uses the same scheme, except the received ciphertext unit is XORed with the *encryption* function's output (not decryption) to recover plaintext.

CFB doesn't quite fit the typical stream-cipher mold: in a standard stream cipher, the keystream depends only on an initial value and the key, then gets XORed with plaintext. In CFB, the stream XORed with the plaintext **also depends on the plaintext itself** (through the feedback of ciphertext into the shift register). One practical consequence: encryption operations can't be parallelized (each depends on the previous result), though decryption *can* be, since the input blocks can be constructed in series from the IV and ciphertext first.

---

## Output Feedback (OFB) mode

OFB is structurally similar to CFB, with one key difference: the feedback is the **output of the encryption function itself**, not the ciphertext — and OFB operates on **full blocks**, not `s`-bit segments.

Because the feedback doesn't depend on plaintext or ciphertext at all, the sequence of encryption outputs `Oi` depends **only on the key and the IV**. This means the IV for OFB **must be a nonce** — unique to every execution — since for a fixed key and IV the output stream is entirely fixed; reusing it with different messages would let an attacker recover the shared portion of the `Oi` stream if any plaintext block repeats across messages.

| | CFB | OFB |
|---|---|---|
| Feedback source | The **ciphertext** unit (`s` bits) | The **encryption output** (full block) |
| Unit size | `s`-bit segments | Full `b`-bit blocks |
| Bit error in ciphertext | Corrupts recovered plaintext **and** propagates via the shift register (extra downstream corruption) | Corrupts only the corresponding recovered plaintext block — **does not propagate** |
| Vulnerability | Less exposed to stream-modification attacks | **More** vulnerable: complementing a ciphertext bit complements the same bit in recovered plaintext, enabling controlled plaintext tampering |
| Stream-cipher fit | Doesn't fit the typical mold (output depends on plaintext) | Fits the typical mold — output is independent of plaintext, generated purely from key + IV |

OFB's classic use case is **stream-oriented transmission over a noisy channel** (e.g., satellite links), precisely because bit errors don't propagate.

---

## What to remember for the exam

Both CFB and OFB convert a block cipher into a stream cipher via feedback, and both use the *encryption* function for decryption too (never the decryption function). CFB feeds back the **ciphertext** in `s`-bit segments — errors propagate through the shift register. OFB feeds back the **encryption output** in full blocks, is independent of the plaintext, needs a nonce IV, doesn't propagate transmission errors, but is more exposed to deliberate bit-flipping attacks on the plaintext.

---

## Exam check

1. What gets fed back into the shift register in CFB — and in OFB?
2. Why must the OFB initialization vector be a nonce, unlike CBC's IV?
3. Compare CFB and OFB on error propagation from a transmission bit error.
4. Which mode is more vulnerable to a message stream modification attack, and why?
