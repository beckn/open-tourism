# AccommodationReservation — v1.0

Booking record attributes carried in `contractAttributes` from `on_confirm` onwards. Captures identifiers, lifecycle status, stay summary, lead guest, and financial details returned by the supply system at reservation time. Updated via `on_status`. Cancellation outcome fields (`cancellationDate`, `cancellationChargeAmount`, `refundAmount`) are populated in `on_cancel`.

Beckn container: `contractAttributes`

## Files

| File | Purpose |
|---|---|
| [https://schema.nfh.global/AccommodationReservation/v1.0/attributes.yaml](https://schema.nfh.global/AccommodationReservation/v1.0/attributes.yaml) | OpenAPI schema envelope (versioned path) |
| [https://schema.nfh.global/AccommodationReservation/v1.0/context.jsonld](https://schema.nfh.global/AccommodationReservation/v1.0/context.jsonld) | JSON-LD context (versioned path) |
| [https://schema.nfh.global/AccommodationReservation/v1.0/vocab.jsonld](https://schema.nfh.global/AccommodationReservation/v1.0/vocab.jsonld) | RDF vocabulary (versioned path) |
| [https://schema.nfh.global/AccommodationReservation/v1.0/profile.json](https://schema.nfh.global/AccommodationReservation/v1.0/profile.json) | Beckn profile descriptor (versioned path) |

## Properties

| Property | Required | Type | Description |
|---|---|---|---|
| `bookingReference` | no | string | Network-level booking reference issued by the supply system. External-facing reference and grievance anchor for cancellation-charge disputes. |
| `agentReference` | no | string | Agent or traveller reference echoed from confirm. Idempotency key — retried confirms with the same value return the original outcome. |
| `networkConfirmationRef` | no | string | Supply-system confirmation number. May be empty at `on_confirm` and populated in subsequent `on_status` calls. |
| `propertyConfirmationRef` | no | string | Confirmation number from the property's own reservation system. May be empty at `on_confirm`. |
| `reservationStatus` | no | string (enum) | Booking lifecycle status: `vouchered`, `on-request`, `failed`, `cancelled`, `rejected`. |
| `bookingDate` | no | string | Date and time the reservation was created by the supply system. |
| `cancellationDeadline` | no | string | Cancellation deadline in ISO 8601. After this point charges apply per binding policy. Must be shown on the booking confirmation. |
| `totalChargeAmount` | no | number | Total amount charged. Must match the `expected_price` sent at confirm. Authoritative amount for settlement. |
| `grossAmount` | no | string | Gross amount as returned by the supply system. |
| `chargeCurrency` | no | string | ISO 4217 currency code of the committed amount. |
| `leadSalutation` | no | string | Salutation of the lead guest. |
| `leadFirstName` | no | string | Given name of the lead guest. |
| `leadLastName` | no | string | Family name of the lead guest. |
| `localPropertyId` | no | string | Stable supply-system property identifier. |
| `propertyName` | no | string | Name of the accommodation property. |
| `propertyAddress` | no | string | Address of the property as recorded on the booking. |
| `propertyPhone` | no | string | Property telephone number for guest contact. |
| `propertyRating` | no | string | Star rating of the property at time of booking. |
| `propertyCountry` | no | string | Country name of the property's location. |
| `propertyCity` | no | string | City of the property's location. |
| `checkInDate` | no | string | Scheduled check-in date (YYYY-MM-DD). |
| `checkOutDate` | no | string | Scheduled check-out date (YYYY-MM-DD). |
| `selectedNights` | no | string | Number of nights for the stay. |
| `totalRooms` | no | string | Total number of rooms booked. |
| `totalAdults` | no | string | Total number of adult guests across all rooms. |
| `totalChildren` | no | string | Total number of child guests across all rooms. |
| `roomDetail` | no | array of object | Per-room booking summary. Each entry carries `roomTypeDescription`, `numberOfRooms`, and `roomType`. |
| `specialRemark` | no | string | Special request echoed with the supply system's standard disclaimer. |
| `contractComments` | no | string | Binding contract remarks from the supply system (e.g. government tax applicability). |
| `cancellationPolicyText` | no | string | Full cancellation policy text as returned in the booking-detail retrieval. Present after `on_status`, not at `on_confirm`. |
| `cancellationDate` | no | string | Date and time the booking was cancelled (ISO 8601). Present in `on_cancel`. |
| `cancellationChargeAmount` | no | number | Charge applied on cancellation per the binding policy. Present in `on_cancel`. |
| `cancellationChargeCurrency` | no | string | ISO 4217 currency code of the cancellation charge. |
| `refundAmount` | no | number | Amount refunded to the traveller after deducting any charges. Present in `on_cancel`. |
| `refundCurrency` | no | string | ISO 4217 currency code of the refund amount. |
