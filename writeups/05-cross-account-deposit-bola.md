# Cross-Account Deposit Actions Through Missing Object-Level Authorization

**CVSS v3.1:** 5.4 (Medium) — *indicative score*

## Vulnerability

Authenticated deposit-action endpoints accepted another user's `depositId` without checking object ownership. The corresponding read endpoint correctly denied access to the foreign deposit, but action endpoints still processed it. The behavior was reproduced across two API generations.

## Evidence

1. Account A created a deposit and recorded its identifier.
2. Account B requested the foreign deposit through `GET /api/BANKING/deposit-status`; the response was `404`.
3. Account B sent `POST /api/BANKING/external-deposit-continue` with Account A's identifier. The response was `200 OK` and contained foreign payment-processing metadata, including invoice ID, amount, and currency.
4. `POST /api/BANKING/deposit-complete` also accepted the foreign identifier and responded with `{"completed":true}`.
5. Equivalent behavior occurred in `/api/v1/banking/ContinueDeposit` and `/api/v1/banking/CompleteDeposit`. Fake identifiers failed with `404` in control tests.

## Impact

Any authenticated user who knew another user's deposit identifier could read payment-processing metadata and invoke deposit continuation/completion actions on that object. A response of `completed:true` was confirmed; actual payment settlement was not independently established.
