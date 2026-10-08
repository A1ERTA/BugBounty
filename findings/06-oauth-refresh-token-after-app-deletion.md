# OAuth Refresh Token Remains Valid After Application Deletion Request

**CVSS v3.1:** 5.4 (Medium) — *indicative score*

## Vulnerability

An OAuth client entered the `deletion_requested` state after a deletion request, but previously issued refresh tokens were not revoked. The token endpoint continued to issue new access tokens for the client despite its pending removal.

## Evidence

1. A controlled OAuth application completed authorization and received a valid `refresh_token`.
2. The tester requested application deletion and verified the `deletion_requested` state.
3. The same refresh token was submitted to `POST /v2/oauth2?action=requesttoken` with `grant_type=refresh_token`.
4. The endpoint issued another valid `access_token`. A deliberately invalid client secret failed as a control, confirming this was not unrestricted token issuance.

## Impact

Possession of a previously issued refresh token could preserve delegated API access after the account owner requested that the OAuth client be removed. This behavior weakens incident response and credential revocation.
