# Signed Messages — Technical Notes

## Challenge Context

- Room: Love at First Breach
- Task: Signed Messages
- Focus: web enumeration + cryptographic key recovery
- Public flag disclosure: intentionally redacted

## Discovery

Interesting routes observed during enumeration:

```text
/about
/dashboard
/debug
/login
/logout
/messages
/register
```

## Vulnerability Model

The application exposes a deterministic seed construction:

```text
{username}_lovenote_2026_valentine
```

The prime derivation observed in the challenge is conceptually:

```python
p = nextprime(int.from_bytes(sha256(seed).digest(), "big"))
q = nextprime(int.from_bytes(sha256(seed + b"pki").digest(), "big"))
```

This is not secure key generation because the prime candidates are predictable.

## RSA Reconstruction

```python
n = p * q
phi = (p - 1) * (q - 1)
e = 65537
d = pow(e, -1, phi)
```

CRT helpers:

```python
dmp1 = d % (p - 1)
dmq1 = d % (q - 1)
iqmp = pow(q, -1, p)
```

## Signature Parameters

The working signing configuration uses:

- RSA
- PSS padding
- MGF1
- SHA-256
- `salt_length=MAX_LENGTH`

## Key Assessment Point

The interface's RSA-2048 wording should not be accepted as evidence of true RSA-2048 implementation. The disclosed derivation starts from 256-bit SHA-256 digests and applies `nextprime()` directly.

## Reproducibility Checklist

- [ ] Identify username accepted by the verification endpoint.
- [ ] Retrieve the exact target message from `/messages`.
- [ ] Reproduce the seed string exactly.
- [ ] Generate the same `p` and `q`.
- [ ] Recompute `n`, `phi`, `d`.
- [ ] Reconstruct the private key.
- [ ] Sign with RSA-PSS/SHA-256 and MAX_LENGTH salt.
- [ ] Submit the exact message and signature to `/verify`.
- [ ] Record verification output.
- [ ] Keep the final challenge flag redacted in public documentation.
