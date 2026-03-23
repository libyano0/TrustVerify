# TrustVerify – Project Report

**Team:** [Your Team Name]  
**Date:** March 2026  
**Tool:** `trustverify.py` – CLI for File Integrity and Digital Signatures

---

## 1. Why Hashing Alone Is Not Enough to Prove Identity

A SHA-256 hash guarantees **integrity**: if even one byte of a file changes, the hash changes. This means a receiver can detect accidental corruption or unauthorized modification of a file.

However, hashing provides **no authenticity**. Consider this attack:

1. Alice sends Bob `metadata.json` + its SHA-256 hash.
2. Mallory (a man-in-the-middle) intercepts both.
3. Mallory replaces `metadata.json` with her own tampered version.
4. Mallory **recomputes** a new SHA-256 of her tampered file and sends that instead.
5. Bob verifies the hash — it matches — and he has no way to know the file came from Mallory, not Alice.

The hash only proves that the file matches *the hash that was sent*. It cannot prove **who** generated that hash. Anyone can compute a SHA-256. There is no secret involved, so there is no proof of origin. This is the **integrity vs. authenticity** gap.

---

## 2. How Public/Private Keys Ensure Non-Repudiation

RSA digital signatures close this gap using asymmetric cryptography:

- Alice generates a **key pair**: a private key (secret, never shared) and a public key (shared openly).
- To sign, Alice computes the SHA-256 of `metadata.json` and **encrypts that hash with her private key** — this encrypted hash is the *signature*.
- To verify, Bob uses Alice's **public key to decrypt** the signature, recovering the hash. He then independently computes the SHA-256 of the received `metadata.json` and compares the two values.

**Why this ensures non-repudiation:**

| Property | Explanation |
|---|---|
| **Only Alice can sign** | The private key never leaves Alice's machine. No one else can produce a valid signature for her public key. |
| **Anyone can verify** | The public key is openly shared. Bob (or any third party) can confirm the signature without any shared secret. |
| **Non-repudiation** | If the signature is valid, Alice cannot later deny having signed it — the signature is cryptographically bound to her private key. |
| **Tamper detection** | If Mallory modifies `metadata.json`, the SHA-256 changes. Bob's computed hash won't match the decrypted hash from Alice's signature → verification fails. Unlike hashing alone, Mallory cannot forge a new valid signature because she doesn't have Alice's private key. |

In `TrustVerify`, we use **RSA-PSS** (Probabilistic Signature Scheme) with SHA-256 — the current recommended RSA padding scheme — which adds cryptographic randomness (salt) to prevent certain attacks like chosen-plaintext attacks that are possible with deterministic RSA-PKCS#1 v1.5.

**Summary:** Hashing provides integrity. Digital signatures provide integrity *and* authenticity *and* non-repudiation — the three pillars of a secure file transfer system.
