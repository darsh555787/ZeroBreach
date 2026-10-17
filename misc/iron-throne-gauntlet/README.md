# The Iron Throne — Iron Throne Gauntlet - SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Misc |
| Difficulty | Insane |
| Points | 939 (1,000 base, 13 solves) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |

## 1. Challenge Description

> Four offerings must be laid before the Iron Throne, each proving mastery of one craft of the realm: a Caesar whisper, a maester's layered chain, a wildling's XOR horn, and a dragon's brittle e=3 seal. Lay them in order, and the throne will acknowledge your right.

## 2. Reconnaissance and Gauntlet Breakdown

This challenge is a multi-stage interactive netcat gauntlet (`nc labs.cyberspacevr.in 30025`) consisting of four sequential cryptographic and puzzle stages that must be solved in real time within a single session:

1. **Stage 1 — Caesar Whisper:** Decrypt a word or phrase encrypted with a Caesar shift.
2. **Stage 2 — Maester's Layered Chain:** Iteratively peel back nested encoding layers (for example, stacked Base64, hexadecimal, or URL encodings) to recover the plaintext.
3. **Stage 3 — Wildling's XOR Horn:** Recover plaintext encrypted with a single-byte XOR key, using brute force or frequency analysis.
4. **Stage 4 — Dragon's Brittle e=3 Seal:** Decrypt a textbook low-exponent RSA ciphertext with `e = 3` and weak or no padding by extracting the integer cube root, when the ciphertext is an exact cube.

## 3. Exploitation and Solution Strategy

1. **Automated connection:** Use a persistent TCP client connection to interact with the service and read prompts dynamically.
2. **Solve Stage 1:** Receive the Caesar ciphertext, determine or brute-force the shift, and submit the decoded plaintext.
3. **Solve Stage 2:** Iteratively decode the nested layers until reaching the underlying plaintext, then submit it.
4. **Solve Stage 3:** Try single-byte XOR keys and score candidate plaintexts against expected English text; submit the best valid decryption.
5. **Solve Stage 4:** Obtain the ciphertext `c` and modulus `N`. If `c = m³ < N` (as in an unpadded low-exponent RSA instance), compute the exact integer cube root `m = ∛c`, convert it to bytes, and submit the result.

## 4. Flag

```text
ZeroCTF{the_throne_is_won_by_right_V5FNIJ}
```
