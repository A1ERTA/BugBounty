# Unauthenticated GraphQL Access to Reservation Details by UUID

**Category:** Broken Access Control / IDOR / BOLA (reservation lookup)  
**CVSS v3.1:** 3.7 — Low (indicative)  
**Vector:** `CVSS:3.1/AV:N/AC:H/PR:N/UI:N/S:U/C:L/I:N/A:N`

## Vulnerability

A reservation-management GraphQL endpoint returns reservation information to an unauthenticated client when supplied with a valid `reservationUuid`. The request does not require an application account session, bearer token, or evidence that the requester owns the reservation.

A valid reservation identifier can also be used to render the corresponding management page in the context of an unrelated restaurant identifier. The restaurant identifier in the page URL did not prevent the same reservation details from being displayed.

The finding concerns access **with knowledge of an existing UUID**. Guessing or enumerating random UUIDs was not demonstrated.

## Technical evidence

**Endpoint:** `POST /api/graphql`

1. A reservation-management page was opened using a valid `reservationUuid`, without logging in. The page displayed the customer's first name, date, party size, and reservation status.
2. The same `reservationUuid` was supplied with a different restaurant identifier in the page URL. The reservation was still rendered.
3. A GraphQL request issued through a Chromium browser context (Playwright) returned **HTTP 200** and reservation data without an authenticated application session. The response included `customer.customerUuid`, `customer.firstName`, `mealDate`, `partySize`, `status`, and `reservationUuid`.
4. Direct script-based requests could trigger an anti-bot **HTTP 403**. This response was not evidence of an application-level authorization check; the browser-context request succeeded.

### Cancellation workflow — unverified impact

The unauthenticated reservation page exposed a cancellation workflow and the following GraphQL mutation:

```graphql
mutation CancelReservation($reservationUuid: ID!) {
  cancelReservation(reservationUuid: $reservationUuid) {
    restaurantUuid
    reservationUuid
    status
  }
}
```

**The mutation was intercepted and blocked locally before transmission.** Therefore, unauthorized server-side cancellation **was not demonstrated**. The visible cancellation UI alone does not prove that the mutation would succeed.

## Impact and limitations

- **Confirmed:** unauthenticated retrieval of reservation metadata, including a customer first name and internal identifiers, when the requester possesses a valid reservation UUID.
- **Confirmed:** displaying the same reservation under a different restaurant URL context.
- **Not confirmed:** successful cancellation, UUID enumeration, or access without knowledge of a valid UUID.
- **Exposure precondition:** a valid identifier must become known to a third party, for example through a shared or forwarded management link.

Reservation UUIDs can function as possession-based access tokens by design. Whether this behavior violates the intended authorization model depends on the application's expected link-sharing and cancellation semantics. The indicative CVSS score reflects the limited confirmed confidentiality impact, not the untested cancellation scenario.
