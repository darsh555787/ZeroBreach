# ORBIT-10 — Mobile AI/ML RE - SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Mobile / AI-ML / RE |
| Difficulty | Medium |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |
| Attachments | [Google Drive Attachment 1](https://drive.google.com/file/d/1GZYFiDlWDGoiMTLr1ScLedFY8bJOTrIU/view), [Google Drive Attachment 2](https://drive.google.com/file/d/1aUXrjFnFA3JR6b1MmnqSSVha6-RzSVTb/view) |

## 1. Challenge Description

> Six sensors, one infallible model, and a witness hidden in a linear confession — the motion is gone, the mathematics remains.

## 2. Reconnaissance and Analysis

The challenge provides two binary or model attachment bundles representing an on-device machine-learning or sensor-fusion workflow paired with a linear mathematical confession or encrypted envelope.

- **Sensor and model architecture:** The system evaluates six input sensors against a quantized or linear predictive model.
- **Witness and key recovery:** The mathematical constraints and linear relationships reveal the hidden witness value:

  ```text
  7c4a8d09ca3762af2e1c5f9d3b0a6e4f
  ```

- **Ciphertext decryption:** Use the recovered witness-derived key for AES-GCM counter-mode recovery. The provided notes report a minor authentication-tag mismatch in the wrapper; account for that when validating the recovered plaintext.

## 3. Exploitation Strategy

1. **Analyze the attachments:** Inspect the sensor-model structure and encrypted artifacts from the provided Google Drive links.
2. **Recover the witness:** Solve the linear equations and sensor-math constraints to extract the active witness value.
3. **Decrypt the payload:** Derive the decryption key from the witness and recover the final plaintext flag.

## 4. Flag

```text
ZeroCTF{0rB1t10_3ch0e5_1n_th3_s3ns0r_v4ult_qu4nt1z3d_1nf3r3nc3_r3v34l3d_th3_w1tn3ss_8f2c7d3a91e6}
```
