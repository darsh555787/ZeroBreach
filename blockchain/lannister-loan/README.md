# Lannister Loan — SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Blockchain |
| Difficulty | Easy |
| Points | 500 (500 base, 0 solves) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |

## 1. Challenge Description

> Cersei's accountants deployed a fresh loan contract with a thousand ether of treasury. Borrow, repay, redeem — until the books no longer add up.

## 2. Reconnaissance and Vulnerability Analysis

The challenge presents a smart-contract loan system holding 1,000 ETH in treasury.

- **Vulnerability:** An accounting or state-tracking flaw in the relationship between shares and debt. Redeeming shares without correctly resolving or decrementing the associated debt can desynchronize the contract's state.
- **Underflow exploitation:** Depending on how repayment is calculated against the remaining shares, the flaw can underflow the user's share balance and allow excessive redemption from the treasury.

## 3. Exploitation Strategy

1. **Connect and inspect:** Connect to the remote instance at `labs.cyberspacevr.in:30144` and inspect the contract interface and initial balances.
2. **Execute the transaction sequence:**

   ```text
   tx_borrow 10
   tx_redeem 10
   tx_repay 1
   tx_redeem 1001
   ```

   The sequence manipulates the accounting state: redemption changes share tracking without clearing the debt, and the partial repayment triggers the described share-balance underflow.
3. **Retrieve the flag:** Satisfying the win condition outputs the flag.

## 4. Flag

```text
ZeroCTF{lannisters_owe_negative_gold_CDJFZQ}
```
