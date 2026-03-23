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
pip install cryptography

## Example Commands
python trustverify.py hash test.txt
python trustverify.py manifest myfiles
python trustverify.py check myfiles myfiles/metadata.json
python trustverify.py genkeys
python trustverify.py sign myfiles/metadata.json
python trustverify.py verify myfiles/metadata.json signature.bin

## Submission Contents
- Python script
- Brief report
- GitHub repository
- Unlisted YouTube demo link
