# SQL and ORM Error Disclosure in Checkout Payment Method Validation

**Finding status:** Confirmed (reproduced)  
**Category:** Information Exposure / Verbose Database Errors  
**CVSS v3.1:** 3.7 — Low (estimated)  
**Vector:** `CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:L/I:N/A:N`

## Vulnerability

The checkout order-creation endpoint exposes detailed database exceptions when it receives an invalid `payment_type`. An ordinary visitor session with a controlled cart is sufficient to reach the affected code path.

```text
POST /service.php/order/fromCart.json
Content-Type: application/x-www-form-urlencoded

customer[Email]=<controlled_email>&...&payment_type=0
```

Instead of returning a generic validation error, the server includes the generated `UPDATE` statement, database/table and column names, foreign-key constraint identifiers, and the underlying SQLSTATE exception.

## Technical evidence

An invalid payment method caused an error containing SQL fragments equivalent to:

```text
UPDATE sb_cart SET DEF_PAYMENT_ID = :p1, CUSTOMER_ID = :p2, ...
SQLSTATE[23000]: Integrity constraint violation: 1452
FOREIGN KEY (def_payment_id) REFERENCES sb_def_payment(id)
```

Negative, large, string, quotation-mark, array, and boolean-like input variants produced errors. Time-delay and other controlled probes did **not** establish SQL injection; the requests returned without a SQL-dependent timing difference. No unintended order was created during these tests.

## Impact and limits

**Confirmed:** disclosure of internal database structure, payment-related schema details, SQL statement shape, and ORM diagnostics to a visitor.

**Not confirmed:** SQL injection, unauthorized access to records, or order/payment manipulation through this parameter.
