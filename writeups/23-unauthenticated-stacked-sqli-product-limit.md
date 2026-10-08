# Unauthenticated Stacked SQL Injection in a Product Listing Limit Parameter

**Finding status:** Confirmed (reproduced)  
**Category:** SQL Injection / Stacked Queries / Time-Based Inference  
**CVSS v3.1:** 7.5 — High (estimated)  
**Vector:** `CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:N/A:N`

## Vulnerability

The public product-list API concatenates the `limit` query parameter into a SQL statement and accepts semicolon-separated additional statements. This allows an unauthenticated requester to execute SQL expressions and use response delays as an inference channel.

Affected routes:

```text
GET /service.php/sbProductService/list.json?limit=<value>
GET /service.php/sbProductService/list.xml?limit=<value>
```

## Technical evidence

A controlled proof used the following values:

```text
limit=1
limit=1;SELECT SLEEP(0)
limit=1;SELECT SLEEP(1)
limit=1;SELECT SLEEP(2)
```

The API returned normal response bodies while the last two values produced approximately **one-second** and **two-second** additional delays, respectively, on both JSON and XML routes.

Follow-up conditional expressions involving `DATABASE()` yielded a stable timing difference: false predicates responded at baseline speed, whereas true predicates added approximately one second. This demonstrated a blind inference channel, not just a fixed delay. Invalid `limit` syntax also exposed the generated SQL query and database diagnostics.

No user authentication cookies were needed for these requests.

## Impact and limits

**Confirmed:** pre-authentication execution of additional SQL statements, time-based conditional inference about database metadata, and SQL/schema error disclosure.

**Not attempted or confirmed:** dumping customer records, modifying database data, extracting secrets, reading/writing files, or operating-system command execution. The confidentiality rating reflects the potential of the demonstrated blind SQL channel, not completed bulk extraction.
