# Direct Access to Invoice PDF Files Without Authorization

**Finding status:** Confirmed (reproduced)  
**Category:** Broken Access Control / Document Exposure  
**CVSS v3.1:** 5.9 — Medium (estimated)  
**Vector:** `CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:H/I:N/A:N`

## Vulnerability

An administrative invoice registry links directly to generated invoice PDFs stored beneath a web-accessible directory. The PDF URLs can subsequently be requested without an authenticated administrative session, indicating that access controls on the registry are not enforced on the files themselves.

Example URL structure (placeholders only):

```text
/sbInvoicePlugin/pdf/<year>/<month>/<invoice_path>/<variant>/<timestamp>.pdf
```

## Technical evidence

1. The invoice registry was inspected using an authorized administrator session, and its existing PDF links were collected.
2. An unauthenticated `HEAD` request to each of the **15** observed PDF URLs returned `200 OK` with `Content-Type: application/pdf`.
3. A controlled byte-range request on **three** distinct PDFs returned `206 Partial Content`, `Content-Range: bytes 0-31/...`, and the expected `%PDF-1.7` header bytes.
4. Selected routes also returned PDF content without the `.pdf` suffix.

Only headers and the first 32 bytes were requested for the byte-range checks; full invoice documents were not downloaded or inspected.

## Impact and limits

**Confirmed:** a person with a valid direct invoice URL can retrieve PDF bytes without an admin session; all 15 URLs observed in the authorized registry passed the unauthenticated header check.

**Potential:** exposure of billing data stored inside invoices. Specific customer or financial contents were not inspected. Enumeration or guessing of additional invoice URLs was **not** demonstrated.
