# Cross-Session Cart Item Modification and Deletion via Unscoped Identifier

**Finding status:** Confirmed (reproduced)  
**Category:** Broken Access Control / IDOR  
**CVSS v3.1:** 4.8 — Medium (estimated)  
**Vector:** `CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:L/I:L/A:N`

## Vulnerability

Cart-item update and deletion endpoints identify the target item by `cart_id` but do not verify that the item belongs to the requesting visitor session. An unauthenticated visitor who knows another visitor's valid cart-item identifier can modify or delete it using their own independent session.

Affected operations include:

```text
POST /service.php/sbCartService/updateItem.xml?fullCart=1&symfony=<session>
POST /service.php/sbCartService/update.xml?fullCart=1&symfony=<session>
```

## Technical evidence

1. **Session A** added one ticket to a controlled cart, with quantity `1` and a total of `65.00`.
2. **Session B**, created independently, submitted A's `cart_id` to `updateItem.xml` with `quantity=2`.
3. A fresh cart preview under A showed quantity `2` and a total of `130.00`.
4. In a separate controlled run, B used `update.xml` with `deleted[0]=<A_cart_id>`. A's next preview showed the item had been removed.
5. An invalid item ID produced a distinct not-found error, whereas a valid foreign ID returned cart-item details and applied the requested change.

Two controlled sessions received adjacent item identifiers. A cross-session update also succeeded when the request carried a foreign `Origin` header. These observations indicate an identifier-discovery risk, **not** a demonstrated brute-force or browser-based CSRF exploit.

## Impact and limits

**Confirmed:** a visitor with a valid foreign item identifier can read basic item details through the mutation response, change the ticket quantity and cart total, or delete another visitor's cart item.

**Not confirmed:** exploitation without obtaining or predicting a valid `cart_id`, payment manipulation, or issuance of free tickets. A separate negative-quantity behavior was observed but was not carried through order creation.
