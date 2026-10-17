# The Dragon's Hoard — Iron Throne Gauntlet - SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Crypto |
| Difficulty | Hard |
| Points | 798 (800 base, 2 solves) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |

## 1. Challenge Description

> The dragon sealed its hoard with a seal of three: it raised the secret to the third power against a great modulus. But a seal of three is brittle when the secret is small — the seal never wraps around, and a cube is trivially un-cubed.

## 2. Reconnaissance and Vulnerability Analysis

The challenge description points to a textbook low-exponent RSA or unwrapped-cube attack with `e = 3`.

- **Vulnerability:** In textbook RSA, `c = m^e mod N`. If `m` is small enough that `m^e < N`, the modular reduction does not change the value.
- **Exploitation principle:** In that case, `c = m^3`, so the plaintext can be recovered by computing the exact integer cube root `m = ∛c`.

## 3. Exploitation Strategy

1. **Connect and gather parameters:** Connect to the remote netcat service at `labs.cyberspacevr.in:30141` and obtain the public parameters (`e = 3`, modulus `N`, and ciphertext `c`).
2. **Compute the integer cube root:** Calculate the exact integer cube root of `c` locally:

   ```text
   m = ∛c
   ```

3. **Decode the flag:** Convert the resulting integer to bytes to reveal the flag.

## 4. Flag

```text
ZeroCTF{fire_cannot_kill_a_dragon_F558NP}
```
