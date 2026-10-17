# Sigils and Secrets — Iron Throne Gauntlet - SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Web |
| Difficulty | Easy |
| Points | 296 (300 base, 7 solves) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |

## 1. Challenge Description

> The herald fetches any sigil you name from his hall — but heralds are careless, and the hall is not the only room in the keep. The maester's private vault sits just one room up.

## 2. Reconnaissance and Vulnerability Analysis

The challenge presents a web service accessible over HTTP at `labs.cyberspacevr.in:30665`, where a herald fetches named sigil files from a specific directory.

- **Vulnerability:** Path traversal (local file inclusion / directory traversal). User input is passed directly into file-fetching operations without adequate sanitization or directory-boundary enforcement, allowing `../` sequences to escape the public hall directory.
- **Target:** The description's hint ("the maester's private vault sits just one room up") suggests traversing one directory level up to access a restricted file such as `maester_secret`.

## 3. Exploitation Strategy

1. **Test path traversal:** Supply a traversal path (for example, `../maester_secret`, or an equivalent path traversal sequence) when requesting a sigil from the herald service.
2. **Retrieve the secret file:** Read the contents of the maester's private vault file.
3. **Capture the flag:** The retrieved `maester_secret` file contains the hidden flag.

## 4. Flag

```text
ZeroCTF{sigils_reveal_the_hidden_path_YY92WN}
```
