# Iron Throne Bank — SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Blockchain |
| Difficulty | Easy |
| Points | 500 (500 base) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |

## 1. Challenge Description

> The Iron Bank of Braavos has staked a thousand ether in a contract under the watch of a trusting lord treasurer. Audit the source carefully and walk away with the gold.

## 2. Vulnerability Analysis

The challenge features a smart contract vulnerable to classic reentrancy.

- **The flaw:** The withdrawal function transfers ETH to the caller before completing the balance bookkeeping update.
- **The exploit:** A malicious recipient contract can use its `receive()` callback to re-enter the withdrawal function while the original call's state changes are still pending. The recursive calls can bypass balance checks against stale accounting state.

```solidity
// Conceptual vulnerable ordering
require(balances[msg.sender] >= amount);
(bool ok, ) = msg.sender.call{value: amount}("");
require(ok);
balances[msg.sender] -= amount; // State update happens too late
```

## 3. Exploitation Methodology

1. **Inspect state:** Run `state` to inspect the bank total, EOA balance, attacker-contract balance, recorded attacker balance, and remaining re-entry depth.
2. **Register the attacker:** Run `register_attacker 20` to deploy an attacker contract with a receive callback configured for up to 20 re-entries (seeded with 50 ETH).
3. **Deposit:** With the new attacker selected as the sender, execute `tx value=50 deposit`.
4. **Trigger reentrancy:** Execute `tx withdraw 50`. The callback recursively invokes the withdrawal path before the original balance bookkeeping completes.
5. **Verify state:** Run `state` to confirm the bank total reached 0, the attacker contract holds 550 ETH, and recursion executed successfully.
6. **Claim the flag:** Run `flag` to satisfy the win condition (`bank_total == 0` and `your_total_ether >= 100`) and retrieve the flag.

## 4. Final Flag

```text
ZeroCTF{the_iron_throne_drained_again_7A1OXY}
```
