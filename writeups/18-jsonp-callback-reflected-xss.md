# Reflected XSS via an Unvalidated JSONP Callback in Cart Services

**Finding status:** Confirmed (reproduced)  
**Category:** Reflected Cross-Site Scripting (XSS)  
**CVSS v3.1:** 6.1 — Medium (estimated)  
**Vector:** `CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:C/C:L/I:L/A:N`

## Vulnerability

Several cart-service endpoints prepend an unchecked `callback` value to their responses. Because the resulting content is served as `text/html`, an HTML payload supplied in the callback parameter is parsed and executed by the browser instead of being treated as a JavaScript identifier.

## Technical evidence

The following pattern returned an HTML-executable response:

```text
GET /service.php/sbCartService/cart.json?callback=<svg/onload=alert(document.domain)>
```

The server returned a body beginning with the injected `<svg>` element, followed by its JSON/JSONP cart response, under `Content-Type: text/html; charset=utf-8`. `X-Content-Type-Options: nosniff` was not present.

Direct Chromium navigation confirmed JavaScript execution on these three endpoints:

- `/service.php/sbCartService/cart.json`
- `/service.php/sbCartService/save.xml`
- `/service.php/sbCartService/split.json`

A tested `union.xml` variant did **not** reproduce the execution. Injected code could check for the presence of a `symfony` cookie, without reading or publishing its value.

## Impact and limits

**Confirmed:** unauthenticated reflected JavaScript execution in the cart-service origin on three endpoints.

**Important limitation:** unauthenticated responses may set a new `symfony` cookie, replacing an existing same-name cookie before the payload executes. Consequently, theft of an existing privileged session through this exact request path was **not** demonstrated.
