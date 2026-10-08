# Unauthenticated Reflected XSS in a Language-Selection Error Page

**Finding status:** Confirmed (reproduced)  
**Category:** Reflected Cross-Site Scripting (XSS)  
**CVSS v3.1:** 6.1 — Medium (estimated)  
**Vector:** `CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:C/C:L/I:L/A:N`

## Vulnerability

An invalid language-selection query parameter is inserted into an HTML error message without context-appropriate escaping. A crafted link can therefore execute arbitrary JavaScript in the ticketing application's origin without prior authentication.

Affected parameter variants include `culture` and `sf_culture` on the site root and supported front-controller routes.

## Technical evidence

An example URL-encoded proof value is:

```text
/?culture=%22%3E%3Csvg%2Fonload%3Dalert(document.domain)%3E
```

The server returned `200 OK` with the injected element inside its HTML error page. A Chromium browser test confirmed execution of `alert(document.domain)` in the trusted application origin.

A controlled JavaScript check could also detect the presence of the `symfony` session cookie in `document.cookie`, without collecting its value. The observed cookie configuration omitted `HttpOnly`, `Secure`, and `SameSite`; the response also lacked a restrictive Content Security Policy.

## Impact and limits

**Confirmed:** unauthenticated reflected JavaScript execution, same-origin DOM access, and visibility of the non-`HttpOnly` visitor-cookie name from injected script.

**Possible:** phishing through trusted-origin content and actions performed in a victim's browser. Access to an authenticated staff account, theft of a staff cookie, and account takeover were **not** demonstrated.
