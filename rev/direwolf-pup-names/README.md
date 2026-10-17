# Direwolf Pup Names — SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Rev |
| Difficulty | Medium |
| Points | 489 (500 base, 8 solves) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |
| Attachment | `direwolf_pups` |

## 1. Challenge Description

> Lord Stark's children each named one pup. Speak the six names back, four bytes per pup, twenty-four bytes total. Each pup remembers a different test of its name.

## 2. Reconnaissance and Binary Analysis

The challenge provides an ELF executable named `direwolf_pups`.

- **Core logic:** The binary expects an exact 24-byte input consisting of six 4-byte names representing the direwolf pups of Lord Stark's children: Grey Wind, Lady, Nymeria, Summer, Shaggydog, and Ghost.
- **Validation:** Each of the six 4-byte components is validated independently by a different algorithmic test inside the binary, such as character-sum checks, sum-of-squares constraints, or multiset checksums.
- **Reversing approach:** Disassembling the validation routines reveals the target constraints for each pup's 4-byte name, allowing us to derive the required string.

## 3. Exploitation Strategy

1. **Static analysis:** Inspect the disassembly around the validation functions to identify the checks for all six pup names: Grey Wind, Lady, Nymeria, Summer, Shaggydog, and Ghost.
2. **Derive the input:** Reconstruct the exact 24-byte string:

   ```text
   GREYLADYNYMRSUMMSHGGGHST
   ```

3. **Submit to the remote instance:** Connect to the netcat service at `labs.cyberspacevr.in:30554`, provide the 24-byte string, and capture the flag.

## 4. Flag

```text
ZeroCTF{a_pup_for_every_stark_J2M7TF}
```
