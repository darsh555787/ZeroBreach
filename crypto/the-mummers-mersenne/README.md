# The Mummers' Mersenne — SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Crypto / Misc |
| Difficulty | Medium / Hard |
| Points | 480 (500 base, 11 solves) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |
| Attachments | None |

## 1. Challenge Description

> The mummers shuffle their cards with a Mersenne Twister whose seed they refuse to reveal. They will deal you 624 cards face up — one revolution of the deck — before pulling the next from beneath the cloth. Predict the 625th and they will pay you in flag.

## 2. Reconnaissance and Vulnerability Analysis

The challenge uses the standard Mersenne Twister (MT19937) pseudorandom number generator.

- **Vulnerability:** MT19937 is deterministic. Observing 624 consecutive 32-bit outputs provides enough information to reconstruct its internal state vector of 624 words.
- **Exploitation principle:** Apply inverse-tempering (untempering) to each observed output to recover the raw state words. Reconstruct the generator state and advance it to predict subsequent outputs, including the 625th value.

## 3. Exploitation Strategy

1. **Connect and capture:** Connect to the remote lab instance at `labs.cyberspacevr.in:30396` and capture the 624 outputs dealt face up.
2. **Untemper the outputs:** Reverse the MT19937 tempering operations (right shifts, XORs, and masked left shifts) for each observed 32-bit value to recover the 624 raw state words.
3. **Predict the 625th value:** Initialize an MT19937 instance with the recovered state, generate the next value, and submit it to the service to claim the flag.

## 4. Flag

```text
ZeroCTF{the_mummers_dance_predicted_PTA992}
```
