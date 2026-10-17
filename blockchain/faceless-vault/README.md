# Faceless Vault — SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Blockchain |
| Difficulty | Medium |
| Points | 498 (500 base, 3 solves) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |

## 1. Challenge Description

> A vault holds five hundred ether under the watch of a sworn owner. The vault keeps a delegation pact with a separate Library, which any user may invoke via the vault's open forwarder. Read both contracts carefully.

## 2. Reconnaissance and Vulnerability Analysis

The challenge presents a smart-contract setup comprising a Vault holding 500 ETH and a separate Library contract linked through a `delegatecall` or proxy pattern.

- **Vulnerability:** Storage collision / storage-layout overwrite. The vault uses an open forwarder to invoke library functions via `delegatecall` without adequate access control or strict storage-layout alignment. A write in the library can therefore overwrite the vault's slot 0, which stores the owner address.
- **Exploitation principle:** Invoking the library through the vault's open forwarder lets an attacker overwrite the owner slot with their player EOA address.

## 3. Exploitation Strategy

1. **Inspect the contracts:** Connect to the remote instance at `labs.cyberspacevr.in:30874` and inspect the Solidity source for both the Vault and Library.
2. **Overwrite the owner slot:** Use the vault's delegated library-call capability to change the vault's owner variable to the assigned player EOA address.
3. **Withdraw the funds:** With owner privileges, withdraw the vault's 500 ETH balance to satisfy the win condition and retrieve the flag.

## 4. Flag

```text
ZeroCTF{no_one_kept_the_ledger_QYSLPO}
```
