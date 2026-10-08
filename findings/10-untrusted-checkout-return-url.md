# Unvalidated Checkout Return URL Enables Phishing Redirects

**CVSS v3.1:** 2.6 (Low) — *indicative score*

## Vulnerability

A partner-facing payment-link API accepted arbitrary `successUrl` and `cancelUrl` values without validating destination domains. These values became return URLs for a legitimate checkout session, while the resulting payment link could be delivered using the application's ordinary transactional email channel.

## Evidence

1. A limited partner account used a quote owned by the tester.
2. `POST /pre/api-contracts/api/v1/quotes/{quoteId}/payment-link` included an external controlled URL in `successUrl`.
3. The response was `201 Created`; the created checkout session retained the untrusted destination.
4. `POST /pre/api-contracts/api/v1/quotes/{quoteId}/send-payment-link` successfully initiated delivery of the legitimate payment link through email.

## Impact

After completing a legitimate checkout, a user could be redirected to an attacker-controlled website with the credibility of the original payment flow. The test used a controlled destination and did not complete a real payment.
