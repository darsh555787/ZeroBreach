# ORBIT-9 — The Quantized Oracle - SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Reverse Engineering / Cryptography / Mobile Security |
| Difficulty | Hard / Insane |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |
| Attachments | [Google Drive Attachment 1](https://drive.google.com/file/d/1uvYDWl4giardd0WOxN2YQ46nfi1S-TRT/view?usp=sharing), [Google Drive Attachment 2](https://drive.google.com/file/d/1DWROJP6sHNGPEhQ8BC1_vzdD3YahnDkw/view) |

## 1. Challenge Description

> A mobile device's offline safety classifier has been trained to recognize a single authorized witness token. The release bundle contains two quantized model snapshots, an encrypted calibration block, a compact inference runtime, and an authenticated payload. Recover the active model, reconstruct the input pipeline, and determine the token that makes the on-device classifier accept. Use the accepted token to authenticate the sealed payload.

## 2. Reconnaissance and Architecture Analysis

The release bundle consists of an offline Progressive Web App (PWA) runtime containing:

- Two quantized model snapshots (`model_00.q8` and `model_01.q8`).
- An encrypted calibration block.
- A compact inference engine and a sealed payload.

## 3. Exploitation and Solution Strategy

### Calibration decryption

The calibration file is an AES-GCM envelope (`O9C2`). Its decryption key is derived using:

```text
HKDF-SHA256(
  SHA-256(model0) || SHA-256(model1),
  salt,
  info = "ZeroCTF:ORBIT-9:calibration:v2"
)
```

Decrypting the envelope yields the active-model digest (selecting `model_01.q8`), a position permutation, and 32 independent symbol-to-domain maps, each with a domain size of 16.

### Constraint satisfaction

The active model is a weighted pairwise factor graph with 76 factors. Each factor contains a `16 × 16` `uint8` lookup table and fires when:

```text
table[mapped[a] * 16 + mapped[c]] >= 128
```

Because the acceptance threshold equals the sum of all factor weights, every factor must fire. Solving the resulting constraint satisfaction problem (CSP) with Z3 yields a unique domain assignment. Inverting the calibration maps and permutation recovers the 32-hex witness token:

```text
b606e092e83309f25c0b258e88c11bf3
```

### Payload authentication

Using the recovered witness token as input to the HKDF key derivation unlocks the sealed payload (AES-GCM envelope `O9S2`), revealing the flag.

## 4. Flag

```text
ZeroCTF{quantized_oracle_speaks_in_singular_values}
```
