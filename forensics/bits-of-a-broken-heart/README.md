# Bits of a Broken Heart — SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Forensics / Cryptography / Reverse Engineering |
| Difficulty | Hard / Insane |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |
| Attachment | `Heart.zip` |

## 1. Challenge Description

> EverAfter was an encrypted messaging app. Decrypt the sealed final letter.

## 2. Reconnaissance and Protocol Analysis

The challenge provides an archive (`Heart.zip`) containing the source code for the EverAfter encrypted messaging protocol, chat-history JSON records, message metadata, and a sealed final letter.

- **Vulnerability:** ECDSA nonce leakage / side-channel flaw. The custom protocol exposes the top 5 bits of each signing nonce across multiple messages, or reuses them predictably.
- **Exploitation principle:** Collect valid message signatures and their corresponding leaked nonce prefixes, then construct a Hidden Number Problem (HNP) lattice. CVP/SVP methods with `fpylll` can recover the ECDSA private signing scalar.

## 3. Exploitation Strategy

1. **Extract and inspect:** Extract `Heart.zip` and review the message-history JSON records and protocol source code.
2. **Validate signatures:** Verify message signatures against the supplied public key to filter out invalid records.
3. **Recover the ECDSA key:** Build an embedded lattice for the leaked top 5 nonce bits and solve the resulting HNP with lattice reduction (for example, BKZ) to recover the private signing scalar.
4. **Decrypt the final letter:** Use the recovered private scalar with the letter's peer public key and relationship ID to derive the shared ECDH secret. Apply HKDF and AES-GCM to decrypt the sealed letter and extract the flag.

## 4. Flag

```text
ZeroCTF{3v3n_a_br0k3n_h34rt_l34k5_1t5_l45t_b1t5}
```
