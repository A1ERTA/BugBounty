# Role Hierarchy Bypass Allows a Manager to Suspend a Higher-Privilege Account

**CVSS v3.1:** 5.4 (Medium) — *indicative score*

## Vulnerability

An account-management API enforced role permissions inconsistently. A user with a `Manager` role could modify an account assigned the higher `Chief` role, despite not having authority to administer that user.

## Evidence

1. A Manager session listed accounts via `GET /pre/api-partner/api/v1/accounts?role=usr:network&size=100` and identified a controlled Chief account.
2. The Manager issued `PATCH /pre/api-partner/api/v1/accounts/{chiefAccountId}` with `{"state":"SUSPENDED"}`.
3. The API returned `200 OK`; the Chief user's existing session began receiving `403 Forbidden` for `/accounts/me`, and subsequent login failed.
4. A lower `Seller` role could not perform the same change, illustrating the inconsistent role hierarchy enforcement.
5. Changing the Chief profile's email field was also demonstrated. The test account was restored afterward.

## Impact

A less privileged user could suspend a more privileged user's account, alter profile information, and access account metadata. Full account takeover was not demonstrated.
