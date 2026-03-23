# TrustVerify CLI

**Team:** CipherGuard Duo  
**Course Project:** Mini Project I  
**Tool:** `trustverify.py`

## Overview
TrustVerify is a Python CLI tool for file integrity verification and digital signatures using SHA-256 and RSA.

## Features
- Generate SHA-256 hash for any file
- Generate a manifest file (`metadata.json`) for a directory
- Detect file tampering by comparing current files with the manifest
- Generate RSA public/private keys
- Sign the manifest using the private key
- Verify the signature using the public key

## Requirements
```bash
pip install cryptography
