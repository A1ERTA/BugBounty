# Unauthenticated Administrator Escalation via Forwarded-IP Spoofing

**CVSS v3.1:** 9.3 (Critical)

## Vulnerability

An application applied an IP-based access rule using an untrusted client-supplied `X-Forwarded-For` header. A separate account-promotion endpoint, `/enableAdmin`, was also reachable without authentication. Combining both weaknesses allowed an external user to promote a self-created account to the `ADMIN` role.

## Evidence

1. An anonymous request to `/signup` without a forwarded-IP header was denied with `403 Forbidden`.
2. Repeating the request with an allowlisted address in `X-Forwarded-For` bypassed the IP gate and returned `200 OK`.
3. The tester registered a controlled account. The initial account was pending and could not log in.
4. Calling `/enableAdmin?email={controlledEmail}` with the same spoofed header returned a redirect to a success page.
5. The account could then authenticate. The returned JWT contained `authorities: ["ADMIN"]`, and an administrator-only view became accessible.

## Impact

An attacker with no initial credentials could obtain administrative access and interact with user-management functions. Testing used an account created for this purpose; modification of unrelated users' data was not demonstrated.
