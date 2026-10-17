# The Dragonglass Shard — SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Pwn |
| Difficulty | Medium |
| Points | 500 (500 base, 0 solves) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |
| Attachment | `shard` (Linux x86_64 ELF, statically linked, no PIE) |

## 1. Challenge Description

> A wight stands in the snow, ten feet of frozen sinew. Your hand is on a shard of dragonglass, eight inches long. Drive it home where the heart should be — but be careful with the angle: any error overflows into the bone.

## 2. Reconnaissance and Binary Analysis

The challenge provides a 64-bit ELF executable named `shard`. Inspecting its properties reveals:

- **Protections:** The binary is statically linked and has no PIE, so its code addresses are fixed.
- **Vulnerability:** An insecure read or input routine (`read`/`gets`-style buffer handling) copies more input into a local stack buffer than it can hold, allowing a classic stack-based buffer overflow.
- **Target:** The binary contains a precompiled `win` function at a fixed address that prints the flag if control flow is redirected to it.

## 3. Exploitation Strategy

1. **Calculate the offset:** Disassemble the vulnerable function to measure the distance between the input buffer and the saved return address on the stack.
2. **Craft the payload:** Place padding up to the saved instruction pointer (RIP), followed by the absolute address of the `win` function.
3. **Execute against the instance:** Send the return-to-win payload to the remote lab instance (`nc labs.cyberspacevr.in 30277`) to redirect control flow and retrieve the flag.

## 4. Flag

```text
ZeroCTF{dragonglass_breaks_the_dead_D4YXMX}
```
