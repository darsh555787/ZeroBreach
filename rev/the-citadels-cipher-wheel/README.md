# The Citadel's Cipher Wheel — SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Rev |
| Difficulty | Hard |
| Points | 492 (500 base, 7 solves) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |
| Attachments | `wheel.py`, `sealed.bin` |

## 1. Challenge Description

> The archmaester sealed his counsel with the Citadel's cipher wheel. The wheel is no secret — its turning is taught to every novice. The plaintext is still hidden in the sealed bytes you carry. Take the wheel, turn it backwards, bring the words.

## 2. Reconnaissance and Analysis

This reverse-engineering challenge provides two files:

1. `wheel.py`: The Python script responsible for the cipher wheel's rotation and transformation logic.
2. `sealed.bin`: The encrypted ciphertext file (56 bytes) containing the locked payload.

Inspecting `wheel.py` reveals how the cipher wheel scrambles or shifts bytes based on an internal state machine or key schedule. Since the wheel's mechanism is deterministic and visible in the source, reversing its operations allows us to decrypt `sealed.bin` without interacting with the remote netcat instance (`nc labs.cyberspacevr.in 30298`).

## 3. Exploitation Strategy

1. **Analyze the wheel logic:** Read `wheel.py` to identify the encryption function and construct its exact mathematical or bitwise inverse (an `unwheel` or decryption routine).
2. **Decrypt the ciphertext:** Pass `sealed.bin` through the inverse cipher to recover the underlying plaintext passcode:

   ```text
   the-citadel-keeps-no-secrets-from-its-archmaesters
   ```

3. **Submit to the service:** Connect to the remote lab instance, provide the recovered passcode when prompted, and capture the final flag.

## 4. Flag

```text
ZeroCTF{the_citadel_unsealed_NWUR15}
```
