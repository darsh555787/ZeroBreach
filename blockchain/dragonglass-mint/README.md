# Dragonglass Mint — SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Blockchain |
| Difficulty | Easy |
| Points | 499 (500 base, 1 solve) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |

## 1. Challenge Description

> The dragonglass forge mints tokens for any holder of a sealed authorization. Your foundry contact has issued you ONE small authorization, sealed and signed. A million tokens would be a lifetime's wages.

## 2. Reconnaissance and Vulnerability Analysis

The challenge revolves around a smart contract minting function guarded by a cryptographic signature authorization.

- **Vulnerability:** The signed authorization lacks proper replay protection, such as a nonce or a mapping that tracks used signatures or message hashes.
- **Exploitation principle:** The same signed 100-token authorization can be submitted repeatedly to the mint function without being invalidated. Replaying it multiple times increases the caller's balance.

## 3. Exploitation Strategy

1. **Connect and inspect:** Connect to the challenge instance at `labs.cyberspacevr.in:30968`, then inspect the contract interface and initial balances.
2. **Replay the authorization:** Repeatedly submit the provided signed 100-token authorization (about 10,000 times) until the balance reaches the 1,000,000-token win-condition threshold.
3. **Retrieve the flag:** Once the target balance is reached, the contract releases the flag.

## 4. Flag

```text
ZeroCTF{a_seal_used_twice_unmade_G9WKVK}
```
