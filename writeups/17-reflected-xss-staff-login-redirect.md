# Pre-Authentication Reflected XSS in Staff Login Redirect Parameters

**Finding status:** Confirmed (reproduced)  
**Category:** Reflected Cross-Site Scripting (XSS)  
**CVSS v3.1:** 6.1 — Medium (estimated)  
**Vector:** `CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:C/C:L/I:L/A:N`

## Vulnerability

The `u` redirect parameter is rendered inside a hidden input's `value` attribute on multiple privileged login pages without HTML-attribute escaping. An unauthenticated attacker can terminate the attribute and inject executable markup on the site's official administrative login origin.

Affected login surfaces include the `admin.php`, `backend.php`, `cashbox.php`, `organizer.php`, and `distributor.php` front controllers, including their `/vtLogin/login` routes.

## Technical evidence

A controlled request used a URL-encoded payload equivalent to:

```text
/admin.php/vtLogin/login?u="><svg/onload=alert(document.domain)>
```

The resulting HTML contained a broken-out input attribute followed by an attacker-controlled SVG element:

```html
<input type="hidden" id="redirect" value=""><svg/onload=alert(document.domain)>...
```

Chromium execution was confirmed on the five staff-facing surfaces. A separate benign JavaScript test confirmed that the non-`HttpOnly` `symfony` cookie was visible by name in `document.cookie`, without recording the cookie's value.

Static page assets contained code paths referring to Electron APIs; however, no desktop application was available for testing.

## Impact and limits

**Confirmed:** JavaScript execution from a pre-authentication URL on privileged login pages, including access to the login-page origin and browser-visible session-cookie presence.

**Not confirmed:** staff session theft, credential interception, privilege escalation, or Electron/OS command execution. Those outcomes require additional conditions and were not demonstrated.
