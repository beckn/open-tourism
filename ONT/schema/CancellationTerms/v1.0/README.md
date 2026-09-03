# CancellationTerms — v1.0

Binding cancellation and refund terms confirmed at the pre-commit check (`init`) stage, along with price-integrity and booking gate signals required before any commitment is made. Also carries the point-in-time cancellation charge quote returned in `on_cancel` (try mode). Normalises two supply system response formats: Format 1 (hours-based) and Format 2 (datetime-window policy).

Beckn container: `offerAttributes`

## Files

| File | Purpose |
|---|---|
| [https://schema.nfh.global/CancellationTerms/v1.0/attributes.yaml](https://schema.nfh.global/CancellationTerms/v1.0/attributes.yaml) | OpenAPI schema envelope (versioned path) |
| [https://schema.nfh.global/CancellationTerms/v1.0/context.jsonld](https://schema.nfh.global/CancellationTerms/v1.0/context.jsonld) | JSON-LD context (versioned path) |
| [https://schema.nfh.global/CancellationTerms/v1.0/vocab.jsonld](https://schema.nfh.global/CancellationTerms/v1.0/vocab.jsonld) | RDF vocabulary (versioned path) |
| [https://schema.nfh.global/CancellationTerms/v1.0/profile.json](https://schema.nfh.global/CancellationTerms/v1.0/profile.json) | Beckn profile descriptor (versioned path) |

## Properties

| Property | Required | Type | Description |
|---|---|---|---|
| `verifiedTotalAmount` | no | number | Total booking amount verified at pre-commit. Becomes the `expected_price` the CN must pass at confirm. |
| `verifiedTotalCurrency` | no | string | ISO 4217 currency code of the verified total amount. |
| `contractComment` | no | string | Free-text contract remarks from the supply system (property-collected fees payable at check-in). Must be shown to the traveller before commitment. |
| `bookingPermitted` | no | string | Whether the booking may proceed. `yes` = bookable now; `no` = must not proceed. CN must disable confirm when `no`. |
| `unitSoldOut` | no | string | Whether the selected unit has sold out since discovery. `Yes` = sold out; `No` = still available. |
| `statusMessage` | no | string | Human-readable booking gate status message from the supply system. |
| `cancellationHours` | no | integer | Hours before check-in within which the charge applies (Format 1). A negative value or zero is a stop-booking signal. |
| `appliedChargeAmount` | no | number | Cancellation charge amount under the Format 1 policy. |
| `refundabilityLabel` | no | string | Plain-language refundability classification (e.g. Refundable, Non-Refundable). Format 2. |
| `bindingCancellationWindows` | no | array of object | Ordered binding cancellation charge windows (Format 2). Each entry carries `windowStart`, `windowEnd`, and `chargeAmount`. |
| `amendmentTerms` | no | string | Terms governing date or occupancy amendments after confirmation. |
| `noShowTerms` | no | string | Charge or terms that apply when the guest does not arrive and has not cancelled. |
| `priceChanged` | no | string | Whether the price moved between discovery and this pre-commit check. `yes` = changed; `no` = held. Must be surfaced to the traveller. |
| `priceMovementAmount` | no | number | Monetary amount by which the price changed in `verifiedTotalCurrency`. Present when `priceChanged` is `yes`. |
| `cancellationChargeQuote` | no | object | Point-in-time cancellation charge quotation returned in `on_cancel` (try mode). Carries `allowCancel`, `chargeAmount`, `chargeCurrency`, and `quoteMessage`. |
