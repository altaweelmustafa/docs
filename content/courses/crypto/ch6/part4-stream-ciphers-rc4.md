---
title: "Chapter 6, Part 4 – Stream Ciphers: Structure, Design Considerations, RC4 & Feedback Shift Registers"
date: 2026-08-23
description: "The generic stream cipher structure as a pseudorandom one-time pad, the three key design considerations, RC4's design and eventual weaknesses, and feedback-shift-register-based stream ciphers for constrained devices."
tags: [information-security, cryptography, stream-ciphers, rc4, feedback-shift-register, chapter6]
toc: true
weight: 4
---

## Stream ciphers as a pseudorandom one-time pad

A stream cipher can be viewed as the pseudorandom cousin of the one-time pad (Chapter 3 covered the true one-time pad, which needs a genuinely random key as long as the message). A stream cipher instead uses a **short secret key** plus a **pseudorandomly generated bit stream**, computationally indistinguishable from true randomness.

**Generic structure:** a secret internal **state** evolves over time (starting from state `0`); a **state transition function `f`** computes the next state from the current one at each bit-generation step; an **output function `g`** produces the **keystream** `zi` used for encryption/decryption. A secret key `K` initializes the state (and may also feed into `f` directly); some stream ciphers also take an **initialization vector (IV)** alongside `K` to initialize the state. As with block-cipher IVs, a stream cipher's IV need not be secret — but it must be unpredictable and unique.

Block ciphers have traditionally been more widely used, partly because their modes of operation (CTR, OFB, CBC) let them double as stream ciphers when needed. But interest in dedicated stream ciphers has grown for encrypting large volumes of fast streaming data, and for **constrained devices** with limited memory and power — small wireless sensors (IoT) and RFID tags being prime examples.

---

## Stream cipher design considerations

1. **Large period** — the underlying PRNG's output eventually repeats; the longer that period, the harder cryptanalysis becomes (the same logic as a longer Vigenère keyword being harder to break).
2. **Keystream should approximate true randomness** — roughly equal numbers of 1s and 0s, and (if treated byte-wise) all 256 byte values appearing about equally often. The more random-looking the keystream, the more randomized the ciphertext, and the harder cryptanalysis becomes.
3. **Sufficiently long key** — since the PRNG output is conditioned on the key, the key must resist brute force just as a block cipher key would; **at least 128 bits** is desirable with current technology.

A well-designed stream cipher can be as secure as a block cipher of comparable key length, and — when not built on a block cipher — is often faster and needs far less code (RC4 fits in a few lines). This speed advantage has narrowed with efficient AES software and hardware acceleration (e.g., the Intel AES instruction set), but stream ciphers remain attractive for streaming/constrained use cases.

**Important caveat:** unlike block-cipher keys, which can safely be reused across messages under a mode with a fresh IV, **stream cipher keys must not be reused directly across two plaintexts** — if the same key produces the same keystream for two messages, XORing the two ciphertext streams together cancels the keystream and yields the XOR of the two plaintexts, which is often exploitable if the plaintexts have any known structure (text, credit card numbers, etc).

Streaming applications (data channels, browser/web links) tend to favor stream ciphers; block-oriented applications (file transfer, email, databases) tend to favor block ciphers — though either can technically be used for either.

---

## RC4

Designed in 1987 by **Ron Rivest** for RSA Security: a variable-key-size, byte-oriented stream cipher based on a random permutation. It's cheap — 8 to 16 machine operations per output byte — and runs very fast in software. RC4 was used in **WPA** (part of IEEE 802.11 wireless LAN) and is optional in SSH and Kerberos. RSA kept it as a trade secret until it was anonymously leaked to the Cypherpunks mailing list in September 1994.

**Mechanics (brief):** a variable-length key (1–256 bytes) initializes a 256-byte state vector `S` holding a permutation of all values 0–255. Each output byte is produced by selecting one of the 255 entries of `S` in a systematic way, permuting `S` further each time a byte is generated.

### Strength / weaknesses

- A fundamental vulnerability was found in RC4's **key scheduling algorithm**, reducing the effort needed to recover the key.
- Later cryptanalysis exploited biases in the RC4 **keystream** itself to recover repeatedly-encrypted plaintexts.
- As a result, **RFC 7465** prohibits RC4 in TLS, and NIST's TLS guidance also prohibits RC4 for government use.

---

## Feedback shift register (FSR) stream ciphers

Driven by the growth of constrained devices (IoT, etc.), most recently-developed stream ciphers are built on **feedback shift registers**: compact, hardware-friendly, and backed by well-developed theory on the statistical properties of the sequences they produce.

An FSR is a sequence of 1-bit memory cells, each with an input and output line. At each clock tick: the **rightmost (least significant) bit shifts out** as the output bit for that cycle, every other bit shifts one position to the right, and a **new leftmost bit is computed as a function of the other bits in the register**. (When that function is linear, this is a *linear* feedback shift register, LFSR — the basis of many lightweight stream ciphers.)

---

## What to remember for the exam

A stream cipher is a pseudorandom one-time pad: short key + IV feed a state that evolves via `f` and outputs a keystream via `g`, XORed with plaintext. Three design considerations: large period, near-true-randomness of the keystream, and a key of at least 128 bits — and critically, never reuse a stream-cipher key across two plaintexts. RC4 (1987, Rivest) is fast and simple but is now deprecated (RFC 7465) due to key-scheduling and keystream biases. FSR/LFSR-based ciphers are the modern choice for constrained/IoT devices, shifting a bit out each clock cycle while computing a new bit as a function of the register's current contents.

---

## Exam check

1. Name the three internal elements of the generic stream cipher structure (state, transition function, output function).
2. What goes wrong if the same stream-cipher key is used to encrypt two different plaintexts?
3. Why was RC4 ultimately prohibited in TLS?
4. Describe what happens inside a feedback shift register at each clock cycle.
