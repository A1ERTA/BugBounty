# Multipart Boundary Parser Differential Bypasses Scope Validation

**CVSS v3.1:** 7.5 (High) — *indicative score*

## Vulnerability

An edge gateway and a backend server parsed the same multipart request differently when `Content-Type` contained both a standard `boundary` and an RFC 2231-style `boundary*` parameter. The gateway authorized one logical request while the backend acted on another.

## Evidence

A test request used:

```http
POST /v1/support/bundles/preview
Content-Type: multipart/form-data; boundary*=utf-8''app; boundary=edge
```

1. Parts delimited by `--edge` set `scope=public` for the edge validator.
2. Another set of parts delimited by `--app` set `scope=internal` for the backend parser.
3. The edge gateway approved the public export, while the backend selected the internal bundle.
4. The response contained an internal configuration export with a runtime environment file and a test secret named `SUPPORT_EXPORT_TOKEN`.

## Impact

Inconsistent parsing across a security boundary bypassed export-scope restrictions and exposed internal configuration data. The issue was reproduced in a lab environment.
