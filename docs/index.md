---
layout: default
title: "Signed Messages — TryHackMe CTF"
permalink: /
---

# 💌 Signed Messages

## Love at First Breach — TryHackMe

A portfolio-grade technical walkthrough of a cryptographic trust failure in the **LoveNote** web application.

<div class="challenge-meta">

| Property | Value |
|---|---|
| Platform | TryHackMe |
| Room | Love at First Breach |
| Task | Signed Messages |
| Category | Web / Cryptography |
| Difficulty | Medium |
| Core Finding | Deterministic RSA key generation |
| Outcome | RSA-PSS signature forgery |

</div>

## Attack Path

![Attack chain](assets/06-attack-chain.png)

```text
Reconnaissance
     ↓
Directory Enumeration
     ↓
/debug Disclosure
     ↓
Predictable Seed Recovery
     ↓
RSA Private-Key Reconstruction
     ↓
RSA-PSS Signature Forgery
     ↓
/verify Acceptance
```

## Walkthrough

### 01 — Reconnaissance

The target exposes a LoveNote application that advertises signed and verified messaging.

![Room overview](assets/01-room-overview.png)

### 02 — Application Mapping

The message interface provides server-recognized content that becomes important during signature generation.

![LoveNote target](assets/02-lovenote-target.png)

### 03 — Directory Enumeration

Enumeration reveals `/debug`, expanding the attack surface beyond the normal application routes.

![Enumeration](assets/03-web-enumeration.png)

### 04 — Cryptographic Disclosure

The debug output reveals deterministic seed construction and the steps used to derive the RSA factors.

![Debug output](assets/04-deterministic-rsa-debug.png)

### 05 — Key Reconstruction

The seed can be recreated locally, allowing the same RSA parameters to be derived. Because the prime candidates originate directly from SHA-256-sized integers, the effective modulus is substantially weaker than a genuine RSA-2048 implementation.

### 06 — Signature Forgery

The recovered private key is used to sign the exact application message with RSA-PSS and SHA-256.

### 07 — Verification

The forged signature is accepted as authentic by the verifier.

![Verification](assets/05-forged-signature-verification.png)

## Core Finding

The application does not actually protect message authenticity once its deterministic key-generation recipe is exposed.

The security boundary is therefore:

> **Cryptographic implementation + key generation + secrecy of implementation details**, not merely the use of RSA.

## Flag

The final flag is intentionally **redacted** in this public portfolio:

```text
THM{REDACTED_FOR_PUBLICATION}
```

## Mitigations

- Generate RSA keys with a cryptographically secure random source.
- Never derive private keys from usernames or static seed patterns.
- Disable or protect debug endpoints.
- Enforce real modulus-size checks.
- Store private keys using dedicated key-management controls.
- Regression-test the actual cryptographic parameters used by verification.

## Documentation

For the complete technical walkthrough, see:

**[Full Documentation](../Documentation/Documentation.md)**

## Repository

[GitHub Repository](https://github.com/anurag-rvnkr1/Signed-Messages-TryHackMe-Walkthrough)

---

### Author

**Anurag**  
Cybersecurity / CTF Portfolio
