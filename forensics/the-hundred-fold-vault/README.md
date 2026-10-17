# The Hundred-Fold Vault — SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Forensics / Cryptography |
| Difficulty | Medium |
| Points | 492 (or 500 base) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |
| Attachment | [Google Drive archive](https://drive.google.com/file/d/1WIYhY-SVP9Zl2RZi_MRIBv2p32_L9eeO/view?usp=sharing) |

## 1. Challenge Description

> The sealed archive has survived by changing its shape. A damaged index has been recovered beside a single sealed state. Most of the recorded transitions are lies. Follow the state through the archive, reject the false paths, and recover the original evidence.

## 2. Reconnaissance and Analysis

The challenge provides a large ZIP archive containing a damaged index alongside a binary transition table and a sealed state.

- **File layout:** Analysis of the binary data reveals a state graph. Each record consists of 32 bytes split into a 12-byte source ID, a 12-byte destination ID, and 8 bytes of transition metadata.
- **The puzzle:** The data contains more than 550,000 decoy transitions. Using the index's root fingerprint and tracking valid transitions derived from the evolving state hash filters out the false paths.
- **State-update rules:** Tracing the root-to-target path requires uncovering operational rules, including byte/bit rotations, conditional XORs, and state validations, that govern valid transitions through the archive.

## 3. Exploitation Strategy

1. **Extract the archive:** Download and extract the archive to inspect the index and binary transition table.
2. **Trace the graph:** Match the root-state fingerprint against the records to identify authentic path branches and discard decoys.
3. **Replay state transitions:** Replay the valid state updates, validating fingerprints at each step, until reaching the final evidence block.

## 4. Flag

```text
ZeroCTF{one_truth_through_550000_false_locks}
```
