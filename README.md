# Security Research Write-ups

An English-language collection of confirmed, technically verified bug bounty vulnerability reports. Each entry records reproduced evidence, demonstrated impact, and CVSS v3.1 assessments. Confirmation of a vulnerability is distinct from proof of every possible secondary impact.

Organization names, program references, rewards, screenshots, personal data, and triage commentary are excluded.

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
| 14 | [Cross-Session Cart Item Modification and Deletion via Unscoped Identifier](writeups/14-cross-session-cart-item-idor.md) | 4.8 · Medium* |
| 15 | [SQL and ORM Error Disclosure in Checkout Payment Method Validation](writeups/15-checkout-sql-error-disclosure.md) | 3.7 · Low* |
| 16 | [Unauthenticated Reflected XSS in a Language-Selection Error Page](writeups/16-reflected-xss-language-error-page.md) | 6.1 · Medium* |
| 17 | [Pre-Authentication Reflected XSS in Staff Login Redirect Parameters](writeups/17-reflected-xss-staff-login-redirect.md) | 6.1 · Medium* |
| 18 | [Reflected XSS via an Unvalidated JSONP Callback in Cart Services](writeups/18-jsonp-callback-reflected-xss.md) | 6.1 · Medium* |
| 19 | [Reflected JavaScript-Context XSS in a Checkout Date Parameter](writeups/19-checkout-selecteddate-javascript-xss.md) | 4.7 · Medium* |
| 20 | [Direct Access to Invoice PDF Files Without Authorization](writeups/20-unauthenticated-invoice-pdf-access.md) | 5.9 · Medium* |
| 21 | [Unauthenticated File Upload Enables Server-Side PHP Execution](writeups/21-unauthenticated-filemanager-php-upload-rce.md) | 9.8 · Critical* |
| 22 | [Authenticated Error-Based SQL Injection in a Reporting Filter](writeups/22-authenticated-payment-filter-error-based-sqli.md) | 6.5 · Medium* |
| 23 | [Unauthenticated Stacked SQL Injection in a Product Listing Limit Parameter](writeups/23-unauthenticated-stacked-sqli-product-limit.md) | 7.5 · High* |

*Scores marked with an asterisk are indicative estimates, not independently standardized ratings.*
