# The Stranger's Coin — SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Rev |
| Difficulty | Medium |
| Points | 489 (500 base, 8 solves) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |
| Attachment | `strangers_coin` |

## 1. Challenge Description

> An old coin lies on the altar of the Stranger. It will speak only to a single sixteen-byte true name. The coin is yours — take it home, look at it from every angle, and bring back the name that opens it.

## 2. Reconnaissance and Binary Analysis

The challenge provides an ELF executable named `strangers_coin`.

- **Core logic:** The binary expects an exact 16-byte true name. It transforms the input through a custom validation routine and compares the resulting bytes against embedded target constants.
- **Reversing approach:** Inspecting the binary's functions and constant data arrays allows us to invert the byte transformation, recover the accepted 16-byte name (`the_stranger_god`), and verify it locally.

## 3. Exploitation Strategy

1. **Static analysis and extraction:** Locate the comparison routine and dump the embedded target validation bytes.
2. **Invert the transformation:** Trace the transformation function and invert the constants to derive the accepted 16-byte input:

   ```text
   the_stranger_god
   ```

3. **Submit to the remote instance:** Connect to the remote netcat service at `labs.cyberspacevr.in:30094`, provide the 16-byte name, and capture the flag.

## 4. Flag

```text
ZeroCTF{a_stranger_paid_in_silver_22KBEN}
```
