# The Raven's Trail — SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Forensics / PCAP |
| Difficulty | Medium |
| Points | 464 (500 base, 15 solves) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |
| Attachment | `ravens-trail.pcap` |

## 1. Challenge Description

> A maester intercepted thirty-odd packets of suspicious raven chatter. Most of it is gossip. A few packets bear an unfamiliar four-letter wax seal and seem to fragment a single message into pieces. Reassemble the message and bring back the name to the live server.

## 2. Reconnaissance and Packet Analysis

The challenge provides a small packet capture file (`ravens-trail.pcap`) containing roughly thirty packets of network chatter.

- **Analysis:** Inspecting the PCAP reveals several packets carrying fragmented data payloads marked by an unfamiliar four-letter wax seal.
- **Reassembly:** Extracting and reassembling the fragmented payloads reveals a ciphered or encoded byte sequence rather than plaintext.
- **Decoding:** Applying keystream analysis, XOR transformations, and pattern recognition to the reassembled payload yields the clue *"a-thousand-eyes-and-one"*, pointing to Bloodraven / Three-Eyed Raven lore.

## 3. Exploitation Strategy

1. **Extract packets:** Open `ravens-trail.pcap` in Wireshark or use a Python script with Scapy to filter and extract the relevant fragmented payload bytes.
2. **Reassemble and decode:** Reconstruct the message fragments in order and decode the ciphertext using the discovered XOR/shift key rules.
3. **Submit to the service:** Connect to the remote lab instance at `labs.cyberspacevr.in:30485`, submit the resolved name or passcode, and capture the flag.

## 4. Flag

```text
ZeroCTF{the_ravens_trail_unfolds_5L2S9W}
```
