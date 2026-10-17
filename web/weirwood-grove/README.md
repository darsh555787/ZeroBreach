# Weirwood Grove — SpaceCTF Writeup

| Field | Details |
|---|---|
| Category | Web |
| Difficulty | Insane |
| Points | 999 (1,000 base, 1 solve) |
| Author | Md-Hacker-18 |
| Flag format | `ZeroCTF{...}` |

## 1. Challenge Description

> The grove keeps a roster of runes for the lord's stewards. A steward may render a small message through the grove's template engine. Listen patiently, render carefully, and the grove will whisper.

## 2. Reconnaissance and Vulnerability Analysis

The challenge provides a web application where stewards can submit custom messages rendered by a backend template engine.

- **Vulnerability:** Server-side template injection (SSTI) in Jinja2.
- **Bypasses:** The application uses a strict filter or blacklist that blocks common dangerous keywords and direct attribute lookups, including `__class__`, `__mro__`, and `__subclasses__`.
- **Exploitation principle:** Bypass the filter by obfuscating strings or attributes, then navigate the Jinja template context to access sensitive functionality and read the flag.

## 3. Exploitation Strategy

1. **Authenticate:** Log in as a valid steward and obtain a session cookie that allows message rendering.
2. **Confirm SSTI:** Test the injection point with a harmless mathematical probe such as `{{7*7}}`.
3. **Bypass the blacklist:** Build obfuscated expressions using string slicing or concatenation to avoid blocked tokens while navigating the template context.
4. **Read the flag:** Use the resulting context access to read the `FLAG` environment variable, for example through a suitable process-environment interface.

## 4. Flag

```text
ZeroCTF{the_grove_speaks_only_through_the_steward_77AW56}
```
