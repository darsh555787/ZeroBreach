# Greywind's Trail — SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Rev |
| Difficulty | Medium |
| Points | 499 (500 base, 2 solves) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |
| Attachment | `greywind` |

## 1. Challenge Description

> Robb's pup ran in the dark, leaving thirty-two prints. Each print remembers what came before. Walk the trail with the right thirty-two-byte name and Greywind will know you.

## 2. Reconnaissance and Binary Analysis

The challenge provides an ELF executable named `greywind`. Static and dynamic analysis reveal the following:

- **File type:** 64-bit ELF executable.
- **Core logic:** The binary expects an exact 32-byte input string. Rather than using a standard string comparison or complex cryptography, it processes the input through a rolling-hash checksum verification chain with per-byte constants and a fixed seed.
- **Reversing approach:** Because the rolling hash is deterministic and invertible, we can trace back or solve the constraint equations for each byte of the 32-byte input instead of attempting an infeasible brute-force search.

## 3. Exploitation Strategy

1. **Static analysis:** Disassemble the validation function to extract the rolling-hash constants and determine how each of the 32 input bytes contributes to the final hash.
2. **Input recovery:** Derive the correct 32-byte string from the hash constraints:

   ```text
   greywind_runs_alone_in_the_north
   ```

3. **Remote submission:** Connect to the netcat service (`labs.cyberspacevr.in 30513`), supply the recovered 32-byte string, and capture the flag.

## 4. Flag

```text
ZeroCTF{greywind_runs_alone_V0ZXDV}
```
