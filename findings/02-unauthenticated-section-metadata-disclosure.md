# Unauthenticated Section Metadata and Instructor Identifier Disclosure

**CVSS v3.1:** 6.9 (Medium)

## Vulnerability

An educational platform's section API returned instructor and course metadata **without requiring authentication**. Section codes were published in publicly accessible course syllabi, providing a direct path to real records without guessing identifiers.

## Evidence

The following API routes exposed the same type of data:

```http
GET /api/v1/sections/code/{sectionCode}
GET /api/v1/sections/{sectionXid}
```

- A valid publicly documented section code returned `200 OK` with the instructor's first and last name, internal `xid`, `rmsUserId`, section and organization identifiers, product information, and registration metadata.
- A second independently published section code produced the same type of response.
- A nonexistent code returned `404`, confirming that the behavior was specific to actual records.

## Impact

Unauthenticated users could collect instructor identities, link them to internal account IDs and organizations, and enumerate the existence of sections referenced in public documents. No access to student records or account modification was demonstrated.
