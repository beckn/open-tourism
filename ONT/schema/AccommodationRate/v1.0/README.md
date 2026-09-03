# AccommodationRate — v1.0

Live availability and pricing attributes for a specific room-rate combination, returned in the dynamic `on_discover` response. Valid only within the 20-minute search session that produced it. Binding cancellation terms are confirmed separately at the `init` stage; the indicative windows in this schema are for display only.

Beckn container: `offerAttributes`

## Files

| File | Purpose |
|---|---|
| [https://schema.nfh.global/AccommodationRate/v1.0/attributes.yaml](https://schema.nfh.global/AccommodationRate/v1.0/attributes.yaml) | OpenAPI schema envelope (versioned path) |
| [https://schema.nfh.global/AccommodationRate/v1.0/context.jsonld](https://schema.nfh.global/AccommodationRate/v1.0/context.jsonld) | JSON-LD context (versioned path) |
| [https://schema.nfh.global/AccommodationRate/v1.0/vocab.jsonld](https://schema.nfh.global/AccommodationRate/v1.0/vocab.jsonld) | RDF vocabulary (versioned path) |
| [https://schema.nfh.global/AccommodationRate/v1.0/profile.json](https://schema.nfh.global/AccommodationRate/v1.0/profile.json) | Beckn profile descriptor (versioned path) |

## Properties

| Property | Required | Type | Description |
|---|---|---|---|
| `unitCategory` | no | string | Room or unit type label as defined by the property (e.g. Deluxe Room Twin). |
| `unitTypeDescription` | no | string | Full descriptive label of the room-rate combination including meal basis, pax count, and any discount or refundability labels. |
| `mealBasisCode` | no | string | Short meal-plan code (RO, BB, HB, FB, AI). |
| `mealBasisLabel` | no | string | Human-readable meal plan description as returned by the supply system. |
| `totalRateAmount` | no | number | Total price for the full stay duration in the requested currency. |
| `rateCurrency` | no | string | ISO 4217 currency code for all pricing in this offer. |
| `nightlyRates` | no | array of object | Per-night rate breakdown. Each entry carries `date` (DD-MM-YYYY), `dayOfWeek`, and `nightlyRate`. |
| `refundable` | no | boolean | Whether this rate allows cancellation with at least a partial refund. `false` means non-refundable from booking. Binding terms confirmed at `init`. |
| `adultsCount` | no | integer | Number of adults this rate accommodates. |
| `childrenCount` | no | integer | Number of children this rate accommodates. |
| `childAges` | no | array of integer | Ages of children in the searched party for this room. |
| `unitCount` | no | integer | Number of units of this room type included in this rate. |
| `available` | no | integer | Availability flag from the supply system. `1` = available; `0` = not available. |
| `rateNote` | no | string | Additional note from the supply system about this rate. |
| `indicativeCancellationWindows` | no | array of object | Indicative (non-binding) cancellation charge windows for display at discovery. Each entry carries `windowStart`, `windowEnd`, and `chargeAmount`. Never use for actual charges. |
| `sessionExpiresAt` | no | string | Expiry timestamp of the 20-minute search session. The CN must complete select, init, and confirm before this time. |
