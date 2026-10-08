# Security Research Write-ups

An English-language collection of anonymized bug bounty reports. All entries describe submitted security findings, observed evidence, demonstrated impact, and CVSS v3.1 ratings.

Organization names, reward details, report links, screenshots, personal data, and triage conversations are excluded.

## Reports

| # | Vulnerability | CVSS v3.1 |
|---:|---|---|
| 01 | [Second-Order Liquid Template Injection in Invitation Emails](writeups/01-second-order-liquid-template-injection.md) | 5.3 · Medium |
| 02 | [Unauthenticated Section Metadata and Instructor Identifier Disclosure](writeups/02-unauthenticated-section-metadata-disclosure.md) | 6.9 · Medium |
| 03 | [Unauthenticated Administrator Escalation via Forwarded-IP Spoofing](writeups/03-ip-header-spoofing-admin-escalation.md) | 9.3 · Critical |
| 04 | [DOM XSS Through postMessage and Unrestricted Dynamic import()](writeups/04-postmessage-dynamic-import-dom-xss.md) | 6.1 · Medium* |
| 05 | [Cross-Account Deposit Actions Through Missing Object-Level Authorization](writeups/05-cross-account-deposit-bola.md) | 5.4 · Medium* |
| 06 | [OAuth Refresh Token Remains Valid After Application Deletion Request](writeups/06-oauth-refresh-token-after-app-deletion.md) | 5.4 · Medium* |
| 07 | [Android Package-Name Spoofing Exposes a Private Media Library](writeups/07-android-package-name-spoofing.md) | 4.7 · Medium* |
| 08 | [Client-Controlled Pricing Fields Affect the Checkout Amount](writeups/08-client-side-price-tampering.md) | 6.5 · Medium* |
| 09 | [Role Hierarchy Bypass Allows a Manager to Suspend a Higher-Privilege Account](writeups/09-role-hierarchy-authorization-bypass.md) | 5.4 · Medium* |
| 10 | [Unvalidated Checkout Return URL Enables Phishing Redirects](writeups/10-untrusted-checkout-return-url.md) | 2.6 · Low* |
| 11 | [Unauthenticated GraphQL Access to Reservation Details by UUID](writeups/11-unauthenticated-graphql-reservation-disclosure.md) | 3.7 · Low* |
| 12 | [Android Intent Injection Grants Access to a Privileged WebView Bridge](writeups/12-android-intent-privileged-webview-bridge.md) | 4.4 · Medium* |
| 13 | [WebView Download Handler Leaks an Active Session Token to an External Host](writeups/13-webview-download-session-token-exposure.md) | 7.1 · High* |

*Scores marked with an asterisk are indicative estimates.*
