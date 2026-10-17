# The Crow's Parity — SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Crypto |
| Difficulty | Medium |
| Points | 489 (500 base, 8 solves) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |

## 1. Challenge Description

> A corrupted crow will tell you, for any sealed number you bring it, only one bit — whether the hidden plaintext is even or odd. One bit at a time, across the bit-length of the modulus, is enough to betray everything.

## 2. Reconnaissance and Vulnerability Analysis

The challenge title and description point to a classic **RSA parity oracle attack** (also known as an LSB oracle attack).

In textbook RSA, a ciphertext is computed as `c = m^e mod N`. If an oracle accepts modified ciphertexts and reveals whether the corresponding decrypted plaintext is even or odd, the attacker can recover the original plaintext with roughly `log₂(N)` oracle queries.

### Oracle behavior

Given a modified ciphertext `c'`, the oracle reports the parity of its decrypted plaintext `m'`.

### Mathematical principle

Multiplying a ciphertext by `2^e mod N` multiplies its decrypted plaintext by 2 modulo `N`. Repeatedly applying this transformation and observing the parity lets us narrow the interval containing the original plaintext, much like a binary search.

## 3. Exploitation Strategy

1. **Connect and gather parameters:** Connect to `labs.cyberspacevr.in:30772` and obtain the public key parameters `e` and `N`, along with the target ciphertext `c`.
2. **Query the parity oracle:** Start with bounds `L = 0` and `R = N`. Repeatedly multiply the ciphertext by `2^e mod N`, query the oracle, and use each parity response to halve the candidate interval.
3. **Recover the flag:** Once the interval converges, convert the recovered plaintext integer to bytes to reveal the flag.

## 4. Exploit Script

The following is the connection skeleton from the original notes. It is **not a complete exploit**: parameter parsing and the parity-oracle recovery loop still need to be implemented.

```python
from pwn import *

# Connect to the remote instance
host = "labs.cyberspacevr.in"
port = 30772

r = remote(host, port)

# TODO: Parse N, e, and ciphertext c from the banner/prompts.
# TODO: Implement the binary-search / parity-oracle recovery loop.

# r.interactive()
```

## 5. Flag

```text
ZeroCTF{a_crow_betrays_one_bit_1366HZ}
```
