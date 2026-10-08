# Reflected JavaScript-Context XSS in a Checkout Date Parameter

**Finding status:** Confirmed (reproduced)  
**Category:** Reflected Cross-Site Scripting (XSS)  
**CVSS v3.1:** 4.7 — Medium (estimated)  
**Vector:** `CVSS:3.1/AV:N/AC:H/PR:N/UI:R/S:C/C:L/I:L/A:N`

## Vulnerability

The checkout summary page concatenates the untrusted `selectedDate` query parameter into an inline JavaScript string used to construct a cart URL. With a non-empty cart, page initialization invokes the vulnerable handler and evaluates the injected JavaScript.

```text
GET /index.php/order/summary.html?selectedDate=<payload>
```

## Technical evidence

A controlled payload closed the string and jQuery call, then invoked a harmless browser alert:

```javascript
");alert(document.domain);//
```

In the rendered source, the attacker's text escapes the originally intended string context:

```javascript
$('.cart_buy_button').attr('href', "/index.php/order/summary.html?selectedDate=");alert(document.domain);//...
```

The source report included the full generated line. The resulting injected script passed a JavaScript syntax check, and a Chromium test with a controlled cart showed browser alert dialogs on the application origin.

A separate controlled check returned `true` for the presence of the readable `symfony` session cookie in `document.cookie`; the cookie's value was not collected.

## Impact and limits

**Confirmed:** reflected script execution and visibility of a non-`HttpOnly` visitor session cookie when the victim's cart contains at least one item.

**Preconditions:** the victim must open the crafted checkout URL and have a non-empty cart. Staff-account compromise or theft of another user's session was **not** demonstrated.
