# Second-Order Liquid Template Injection in Invitation Emails

**CVSS v3.1:** 5.3 (Medium)

## Vulnerability

A stored event-name field was treated as ordinary user data when saved, but was later interpreted as **Liquid template source** while generating transactional invitation emails. The email renderer executed injected Liquid expressions, control-flow tags, and recipient-aware template helpers rather than displaying them verbatim.

## Evidence

1. An authenticated tester created an event whose name contained `H153 {{ 7 | plus: 7 }} marker`.
2. A staff invitation sent to a controlled mailbox contained the evaluated string `H153 14 marker`. The API continued to store the original, unevaluated input, confirming that execution occurred during email rendering.
3. A Liquid conditional evaluated the recipient's `customer.email`, and template helpers for unsubscribe and browser-view URLs produced recipient-specific values.
4. A Liquid-generated HTML anchor appeared as an active clickable external link inside a legitimate invitation. A controlled click reached a test webhook.

## Impact

An attacker able to control an event name could alter the content of trusted transactional emails, access recipient-related template context, and embed attacker-controlled links for phishing. **Remote command execution, server-side requests, and access to third-party accounts were not demonstrated.**
