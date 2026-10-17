# The Spider's Web — Iron Throne Gauntlet - SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Misc |
| Difficulty | Hard |
| Points | 897 (900 base, 3 solves) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |

## 1. Challenge Description

> Varys makes you pass three gates before his little birds whisper the secret. The answer to one gate becomes the key to the next: decode a coded whisper, untangle a repeating-key XOR, then derive a SHA-256 key to unseal the truth.

## 2. Reconnaissance and Multi-Gate Analysis

This multi-gate interactive challenge (`nc labs.cyberspacevr.in 30593`) requires solving three sequential cryptographic stages where the output of each gate feeds directly into the next:

1. **Gate I — The Coded Whisper:** A Base64 and/or ROT13-encoded ciphertext is provided dynamically per instance. Decoding it yields the first keyword (for example, `kingslanding`).
2. **Gate II — Repeating-key XOR:** Use the answer from Gate I as the key to decrypt the repeating-key XOR ciphertext in the second gate and retrieve the next keyword (for example, `balon`).
3. **Gate III — SHA-256-derived key:** Combine the two previous words to derive a SHA-256 key, then use it to decrypt the final ciphertext and reveal the flag.

## 3. Exploitation and Solution Strategy

1. **Solve Gate I:** Connect to the instance, capture the dynamically generated encoded whisper, decode its Base64/ROT13 layer locally, and submit the plaintext (`kingslanding`).
2. **Solve Gate II:** Capture the repeating-key XOR ciphertext, use the Gate I plaintext as the key, and submit the decoded keyword (`balon`).
3. **Solve Gate III:** Combine the first two words to compute the SHA-256-derived key, decrypt the final payload, and submit the result to the server to capture the flag.

## 4. Flag

```text
ZeroCTF{little_birds_sing_for_me_ZIN53A}
```
