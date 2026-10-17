# Iron Throne Web — SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Web |
| Difficulty | Hard |
| Points | 737 (800 base, 15 solves) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |

## 1. Challenge Description

A small web service runs at the King's Landing court. Stewards have a day-PIN for the side gate; the king has a sealed scroll. Open the side gate, climb to the throne, and read the scroll. The court is impatient with automated tooling — keep your traffic clean.

## 2. Reconnaissance and Discovery

### Information gathering

Checking the standard `/robots.txt` file immediately revealed an exposed path: `/.git/config`.

### Source disclosure

Accessing the Git configuration files disclosed a weak/accidentally included application JWT signing secret:

```text
788f0ecccf5009bd
```

## 3. Exploitation Strategy

### Token forgery

Using the discovered secret, an HS256-signed JSON Web Token (JWT) was forged with administrative privileges (`{"role":"admin"}`).

### Admin panel access

Presenting the forged token bypassed gate authentication and allowed navigation to the protected `/admin` route.

### Byte-by-byte scroll reconstruction

The `/admin` endpoint documented an internal byte-fetching utility at `/admin/flag/byte?i=N`. Because of per-session rate limits, fresh sessions were used to iteratively query all 183 byte positions of the sealed scroll. Reassembling the fetched bytes revealed the flag.

## 4. Flag

```text
ZeroCTF{the_iron_throne_yields_only_to_patience_1WGFXS}
```
