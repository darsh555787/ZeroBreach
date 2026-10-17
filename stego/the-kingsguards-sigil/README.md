# The Kingsguard's Sigil — SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | OSINT / Stego |
| Difficulty | Insane / Easy (varies by track) |
| Points | 459 (500 base, 16 solves) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |
| Attachment | `kingsguard.png` |

## 1. Challenge Description

> The seven-pointed star of the Kingsguard hangs in every chapter house. The white knights say the truth has always lived in the red of the sigil's edges. Bring the true name back to the live server and the Lord Commander will acknowledge you.

## 2. Reconnaissance and Steganography Analysis

The challenge provides a small PNG image named `kingsguard.png` (2.4 KB).

- **Analysis:** The description's hint ("the truth has always lived in the red of the sigil's edges") suggests extracting hidden data from the least significant bits (LSBs) or channel distribution of red-channel pixels along the image edges.
- **Extraction:** Inspecting the PNG chunk layout and analyzing the red-channel bitstream reveals a hidden payload containing the text `valyrian-true-steel`.

## 3. Exploitation Strategy

1. **Analyze image chunks:** Inspect `kingsguard.png` with steganography tools such as `zsteg`, `steghide`, or a custom Python script to examine the red-channel LSB bitstream.
2. **Recover the passcode:** Extract the hidden text embedded in the image edges:

   ```text
   valyrian-true-steel
   ```

3. **Submit to the service:** Connect to the remote lab instance at `labs.cyberspacevr.in:30596`, submit the recovered string as the unlock command, and capture the flag.

## 4. Flag

```text
ZeroCTF{kingsguard_sigil_reversed_HQM0QE}
```
