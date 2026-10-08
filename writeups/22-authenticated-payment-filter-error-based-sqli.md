# Authenticated Error-Based SQL Injection in a Reporting Filter

**Finding status:** Confirmed (reproduced)  
**Category:** SQL Injection / Error-Based Database Disclosure  
**CVSS v3.1:** 6.5 — Medium (estimated)  
**Vector:** `CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N`

## Vulnerability

A staff-accessible summary report concatenates the `paymentTypeId[]` filter into a MySQL `IN (...)` clause without safely binding each value. Supplying a SQL expression in an array element causes the expression to execute and returns attacker-controlled results through verbose database errors.

```text
POST /admin.php/sbRepertoireSumaryRaport
Parameter: paymentTypeId[]
Authentication: authorized staff/report session
```

## Technical evidence

A malformed quote produced an SQL syntax error displaying the input inside the generated payment-type filter:

```sql
PAYMENT_TYPE IN (1')
```

A controlled error-based proof used MySQL's `UPDATEXML()` and a constant marker:

```sql
UPDATEXML(1, CONCAT(0x7e, 'SQLI_MARKER'), 1)
```

The response exposed an `XPATH syntax error` containing `~SQLI_MARKER`. Bounded follow-ups using `VERSION()`, `DATABASE()`, `CURRENT_USER()`, and selected server variables also returned their values through the same error channel.

Related probes did **not** establish a reliable time-based or boolean-blind oracle. Metadata-only checks did not support a claim of unrestricted filesystem operations or direct operating-system command execution under the current database account.

## Impact and limits

**Confirmed:** SQL expression execution by authenticated report users and error-based extraction of database metadata and context variables.

**Not confirmed:** extraction of customer records or credentials, table modification, local file read, or OS command execution. The estimated confidentiality impact describes the SQL injection capability, not a demonstrated bulk data leak.
