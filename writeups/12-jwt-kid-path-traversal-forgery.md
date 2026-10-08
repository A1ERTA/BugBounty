# JWT Key ID Path Traversal Allows Forged Administrator Tokens

**CVSS v3.1:** 9.3 (Critical) — *indicative score*

## Vulnerability

A JWT verifier constructed an HMAC key-file path directly from the user-controlled `kid` header. Path traversal allowed verification to use a predictable file instead of the intended secret key, making it possible to sign a forged administrator token.

## Evidence

The vulnerable key lookup followed this pattern:

```python
with open(f'/tmp/keys/{kid}.txt', 'rb') as key_file:
    return key_file.read()
```

1. A JWT header supplied `{"alg":"HS256","kid":"../README"}`.
2. The path resolved to `/tmp/README.txt`, whose contents were known in the lab.
3. A new JWT containing `{"isadmin":true}` was HMAC-signed using that content.
4. The application accepted the forged token as an administrator token. A subsequent lab-only chain allowed a local file to be read.

## Impact

Arbitrary key selection through the JWT header broke signature trust and enabled unauthorized administrator access. The demonstration depended on a known, readable file being usable as a signing key.
