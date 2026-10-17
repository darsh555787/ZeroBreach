# The Faceless Token — Iron Throne Gauntlet - SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Web |
| Difficulty | Medium |
| Points | 599 (600 base, 2 solves) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |

## 1. Challenge Description

> The House of Black and White issues a signed face (a JWT) to every servant. The inner sanctum opens only to the many-faced-god. You wear a guest's face. The doorman trusts the token to name its own algorithm...
>
> *Valar morghulis. All men must serve.*

## 2. Reconnaissance and Vulnerability Analysis

The challenge uses JSON Web Token (JWT) authentication, with permissions and roles determined by token claims.

- **Vulnerability:** JWT algorithm-confusion or `none`-algorithm vulnerability. The hint suggests the server may trust the `alg` value supplied in an incoming JWT header instead of enforcing a secure, expected algorithm.
- **Exploitation principle:** Modify the guest token's claims to request privileged access and test whether the server accepts an unsigned token (`alg: "none"`) or is vulnerable to a signing-algorithm confusion issue. The exact attack depends on how the challenge validates tokens.

## 3. Exploitation Strategy

1. **Inspect the guest token:** Capture the guest JWT provided during normal interaction with the service.
2. **Modify token claims:** Change the payload to include the privileged role or claim required to access the inner sanctum.
3. **Test algorithm validation:** Determine whether the service accepts an unsigned token with `alg: "none"` or has a symmetric/asymmetric algorithm-confusion flaw, then craft the token accordingly.
4. **Submit and retrieve the flag:** Send the crafted token to the remote challenge instance at `labs.cyberspacevr.in:30416` to attempt access to the inner sanctum and retrieve the flag.

## 4. Flag

```text
ZeroCTF{no_one_wears_a_false_face_G9MBQH}
```
