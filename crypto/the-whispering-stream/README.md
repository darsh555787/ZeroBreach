# The Whispering Stream — SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Crypto |
| Difficulty | Medium / Hard |
| Points | 498 (500 base, 3 solves) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |

## 1. Challenge Description

> A maester publishes the start of a binary stream and a ciphered block. Recover the message.

## 2. Reconnaissance and Cryptanalysis

The challenge provides a netcat service at `labs.cyberspacevr.in:30362` that leaks the beginning of a pseudorandom binary stream alongside a ciphered block.

- **Linear feedback shift register (LFSR) / linear recurrence:** The 512-bit stream leak reveals a degree-128 linear recurrence governing stream generation.
- **Berlekamp–Massey algorithm:** Apply the Berlekamp–Massey algorithm to the leaked prefix to calculate the minimal polynomial and recurrence relation.
- **Keystream offset:** The ciphertext uses a keystream offset (for example, advanced by 5,000 bits). Advancing the LFSR state by this offset produces the keystream needed to decrypt the ciphertext block with XOR.

## 3. Exploitation Strategy

1. **Connect and capture:** Connect to the remote service and capture the 512-bit stream-prefix leak and ciphertext block.
2. **Reconstruct the recurrence:** Feed the leaked bits into the Berlekamp–Massey algorithm to determine the linear recurrence.
3. **Advance the stream and decrypt:** Advance the stream generator by the advertised ciphertext offset (5,000 bits), XOR the resulting keystream bits with the ciphertext, and convert the output bytes into the flag.

## 4. Flag

```text
ZeroCTF{the_stream_remembers_all_2878ZT}
```
