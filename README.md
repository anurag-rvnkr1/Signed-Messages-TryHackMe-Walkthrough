# Signed Messages — TryHackMe CTF Walkthrough

> **Room:** Love at First Breach — Signed Messages  
> **Category:** Web / Cryptography  
> **Difficulty:** Medium  
> **Platform:** TryHackMe  
> **Focus:** Deterministic key generation, weak cryptographic design, RSA key reconstruction, RSA-PSS signature forgery

## Overview

This repository contains a portfolio-oriented walkthrough of the **Signed Messages** challenge from TryHackMe's **Love at First Breach** room.

The application presents itself as a PKI-backed messaging platform where messages are digitally signed and verified. The intended security boundary fails because sensitive debugging information exposes how the server deterministically derives its RSA key material.

The write-up follows the complete attack path:

`Reconnaissance → Web Enumeration → Debug Disclosure → Seed Recovery → RSA Reconstruction → Signature Forgery → Verification`

## Key Lessons

- Never expose cryptographic implementation details through production debug endpoints.
- A deterministic seed is not a secure source of cryptographic randomness.
- Cryptographic strength claims in application UI must be backed by the implementation.
- RSA key sizes and prime generation must be validated from the actual implementation, not marketing text.
- Signature verification is only as trustworthy as the private-key lifecycle and key-generation process behind it.

## Repository Layout

```text
.
├── Documentation/
│   └── Documentation.md
├── Resources/
│   └── notes.md
├── Screenshots/
│   ├── 01-room-overview.png
│   ├── 02-lovenote-target.png
│   ├── 03-web-enumeration.png
│   ├── 04-deterministic-rsa-debug.png
│   ├── 05-forged-signature-verification.png
│   └── 06-attack-chain.png
├── docs/
│   ├── index.md
│   └── assets/
│       ├── 01-room-overview.png
│       ├── 02-lovenote-target.png
│       ├── 03-web-enumeration.png
│       ├── 04-deterministic-rsa-debug.png
│       ├── 05-forged-signature-verification.png
│       ├── 06-attack-chain.png
│       └── css/
├── _config.yml
└── README.md
```

## Flag Policy

The final flag is intentionally **redacted** from this public repository to keep the write-up useful for learning without publishing the challenge answer verbatim.

## Responsible Use

All testing described here is intended for the authorized TryHackMe challenge environment only. Do not apply the workflow to systems you do not own or lack explicit permission to assess.

## Author

**Anurag**  
GitHub: [anurag-rvnkr1](https://github.com/anurag-rvnkr1)
