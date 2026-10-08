# Security Research Write-ups

A collection of concise, anonymized technical write-ups covering real-world security findings and controlled security labs.

Each entry describes the vulnerability, the evidence collected, the demonstrated impact, and a CVSS v3.1 score. Indicative scores are labeled as such. Company identities, report references, reward information, triage conversations, screenshots, and personal data are intentionally excluded.

## Real-world findings

| # | Finding | CVSS v3.1 |
|---:|---|---|
| 01 | [Second-Order Liquid Template Injection in Invitation Emails](findings/01-second-order-liquid-template-injection.md) | 5.3 · Medium |
| 02 | [Unauthenticated Section Metadata and Instructor Identifier Disclosure](findings/02-unauthenticated-section-metadata-disclosure.md) | 6.9 · Medium |
| 03 | [Unauthenticated Administrator Escalation via Forwarded-IP Spoofing](findings/03-ip-header-spoofing-admin-escalation.md) | 9.3 · Critical |
| 04 | [DOM XSS Through postMessage and Unrestricted Dynamic import()](findings/04-postmessage-dynamic-import-dom-xss.md) | 6.1 · Medium* |
| 05 | [Cross-Account Deposit Actions Through Missing Object-Level Authorization](findings/05-cross-account-deposit-bola.md) | 5.4 · Medium* |
| 06 | [OAuth Refresh Token Remains Valid After Application Deletion Request](findings/06-oauth-refresh-token-after-app-deletion.md) | 5.4 · Medium* |
| 07 | [Android Package-Name Spoofing Exposes a Private Media Library](findings/07-android-package-name-spoofing.md) | 4.7 · Medium* |
| 08 | [Client-Controlled Pricing Fields Affect the Checkout Amount](findings/08-client-side-price-tampering.md) | 6.5 · Medium* |
| 09 | [Role Hierarchy Bypass Allows a Manager to Suspend a Higher-Privilege Account](findings/09-role-hierarchy-authorization-bypass.md) | 5.4 · Medium* |
| 10 | [Unvalidated Checkout Return URL Enables Phishing Redirects](findings/10-untrusted-checkout-return-url.md) | 2.6 · Low* |

## Controlled lab case studies

| # | Case study | Indicative CVSS v3.1 |
|---:|---|---|
| 01 | [ZIP Extraction Symlink Traversal Leading to Server-Side Code Execution](labs/01-zip-symlink-path-traversal-rce.md) | 10.0 · Critical* |
| 02 | [JWT Key ID Path Traversal Allows Forged Administrator Tokens](labs/02-jwt-kid-path-traversal-forgery.md) | 9.3 · Critical* |
| 03 | [ERB Server-Side Template Injection Through an Email Parameter](labs/03-erb-template-injection-in-email.md) | 9.8 · Critical* |
| 04 | [UTF-8 Byte-Length Mismatch Enables Session and ORM Query Injection](labs/04-utf8-cookie-validation-orm-injection.md) | 6.5 · Medium* |
| 05 | [Multipart Boundary Parser Differential Bypasses Scope Validation](labs/05-multipart-boundary-parser-differential.md) | 7.5 · High* |

*Scores marked with an asterisk are indicative estimates, not independently standardized ratings.*
