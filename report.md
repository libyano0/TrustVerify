# TrustVerify – Project Report

**Team:** CipherGuard Duo  
**Members:**  
- Ahmed Esmaeel — 210208964
- Mariam Mohamed - 210208770
- Arwa abouelseoud - 220208732


**Date:** March 2026  
**Tool:** `trustverify.py` – CLI for File Integrity and Digital Signatures

---

## 1. Why Hashing Alone Is Not Enough to Prove Identity

A SHA-256 hash guarantees **integrity**: if even one byte of a file changes, the hash changes. This allows the receiver to detect accidental corruption or unauthorized modification.

However, hashing alone does **not** provide **authenticity**. Consider the following attack:

1. Alice sends Bob `metadata.json` and its SHA-256 hash.
2. Mallory intercepts both.
3. Mallory replaces `metadata.json` with a tampered version.
4. Mallory recomputes the SHA-256 hash of the tampered file.
5. Bob receives the tampered file and the new hash, verifies them, and sees that they match.

In this case, Bob can confirm that the file matches the hash he received, but he still cannot prove that the file actually came from Alice. This is because **anyone** can compute a SHA-256 hash. There is no secret involved and no cryptographic proof of origin.

Therefore, hashing alone provides **integrity**, but not **identity** or **authenticity**.

---

## 2. How Public/Private Keys Ensure Non-Repudiation

RSA digital signatures solve this problem using asymmetric cryptography:

- Alice generates a **key pair** consisting of a private key and a public key.
- The **private key** is kept secret.
- The **public key** is shared openly.

To sign the manifest, Alice signs the hash of `metadata.json` using her **private key**.  
To verify the signature, Bob uses Alice’s **public key** to check that the signature is valid and that it matches the received `metadata.json`.

This provides several security guarantees:

| Property | Explanation |
|---|---|
| **Only Alice can sign** | Only the holder of the private key can generate a valid signature. |
| **Anyone can verify** | Anyone with Alice’s public key can verify the signature. |
| **Non-repudiation** | If the signature is valid, Alice cannot reasonably deny signing the file because the signature is linked to her private key. |
| **Tamper detection** | If the manifest is modified after signing, the signature verification fails. |

In `TrustVerify`, RSA-PSS with SHA-256 is used as the digital signature scheme. This is a modern and secure RSA signature method.

**Summary:** Hashing provides integrity, while digital signatures provide integrity, authenticity, and non-repudiation.

---

## Conclusion

This project shows that hashing is useful for detecting whether a file has changed, but it cannot prove who sent the file. By combining SHA-256 hashing with RSA digital signatures, `TrustVerify` can both detect tampering and verify that the manifest was signed by the legitimate sender.
