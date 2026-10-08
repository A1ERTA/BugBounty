# Security Research Write-ups

A collection of anonymized technical vulnerability write-ups.

Each entry includes technical details, proof of concept, demonstrated impact, and CVSS scoring.

## Vulnerabilities

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
| 11 | [ZIP Extraction Symlink Traversal Leading to Server-Side Code Execution](writeups/11-zip-symlink-path-traversal-rce.md) | 10.0 · Critical* |
| 12 | [JWT Key ID Path Traversal Allows Forged Administrator Tokens](writeups/12-jwt-kid-path-traversal-forgery.md) | 9.3 · Critical* |
| 13 | [ERB Server-Side Template Injection Through an Email Parameter](writeups/13-erb-template-injection-in-email.md) | 9.8 · Critical* |
| 14 | [UTF-8 Byte-Length Mismatch Enables Session and ORM Query Injection](writeups/14-utf8-cookie-validation-orm-injection.md) | 6.5 · Medium* |
| 15 | [Multipart Boundary Parser Differential Bypasses Scope Validation](writeups/15-multipart-boundary-parser-differential.md) | 7.5 · High* |
| 16 | [Unauthenticated GraphQL Access to Reservation Details by UUID](writeups/16-unauthenticated-graphql-reservation-disclosure.md) | 3.7 · Low* |

*Scores marked with an asterisk are indicative estimates.*
