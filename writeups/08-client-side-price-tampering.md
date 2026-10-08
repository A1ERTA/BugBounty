# Client-Controlled Pricing Fields Affect the Checkout Amount

**CVSS v3.1:** 6.5 (Medium) — *indicative score*

## Vulnerability

A public simulation API accepted modifications to server-calculated pricing and fee fields. Downstream quote generation and checkout processes trusted the modified simulation instead of recomputing the amount on the server.

## Evidence

1. An anonymous test simulation was created with an annual price of approximately EUR 38.18 plus a EUR 5 fee.
2. A `PATCH /pre/api-frontoffice/api/v1/simulations/{simulationId}` request replaced `estimation.options[].yearly.price` with `10` and set `feesApplicationAmount.partnerAmount` to `0`.
3. The API retained those values, and an authorized partner flow generated a quote from the modified simulation.
4. A test checkout session used **EUR 10.00 (1,000 cents)** rather than the unmodified **EUR 43.18 (4,318 cents)**. No real payment was completed.

## Impact

An attacker could influence the amount used to create a legitimate checkout session, undermining pricing integrity and partner fee calculations. The observed effect was confirmed in a controlled test flow, not a completed financial transaction.
