# Open Tourism Network v1.0 Schema Pack

**Protocol Version:** 2.0
**Semantic Model:** generalised (Resource / Offer / Contract)
**Schema Pack Version:** 1.0.0
**Domain:** Open Tourism Network — Hotel and Accommodation Booking

## Overview

This schema pack provides the complete set of v1.0 generalised extension schemas for the accommodation booking domain on the Beckn Protocol. It covers hotel and property-based stays using the standard Resource / Offer / Contract / Commitment model, where a single static resource (accommodation unit) is priced live as a rate offer, committed with guest details, and contracted as a reservation record.

## Schema Taxonomy

| Schema | Container | Beckn Stage | Purpose |
|--------|-----------|-------------|---------|
| AccommodationResource | `resourceAttributes` | `catalog/publish`, `on_discover` (static) | Static catalog attributes for an accommodation unit type at a property — property identity, amenities, images, contact |
| AccommodationRate | `offerAttributes` | `on_discover` (dynamic) | Live pricing and availability for a room-rate combination within a 20-minute search session |
| CancellationTerms | `offerAttributes` | `on_init`, `on_cancel` | Binding cancellation and refund terms, booking gate signals, and cancel charge quotations |
| AccommodationReservation | `contractAttributes` | `on_confirm`, `on_status`, `on_cancel` | Booking lifecycle record — identifiers, status, financial summary, stay details, lead guest, cancellation outcome |
| GuestManifest | `commitmentAttributes` | `confirm` | Per-room guest identity under purpose-bound consent — passengers, special request, consent artifact |

## Inheritance Hierarchy

```
resourceAttributes:
  AccommodationResource          (static catalog; published via catalog/publish)

offerAttributes:
  AccommodationRate              (dynamic; valid for 20-minute search session only)
  CancellationTerms              (binding; confirmed at init; cancel quote at on_cancel)

contractAttributes:
  AccommodationReservation       (booking record; created at on_confirm; updated via on_status)

commitmentAttributes:
  GuestManifest                  (PII; sent at confirm only; never in on_discover or on_select)
```

## Booking Flow Mapping

| Beckn Action | Schema(s) Active | Notes |
|---|---|---|
| `catalog/publish` | AccommodationResource | Static unit data published on cron schedule |
| `on_discover` (static) | AccommodationResource | Property and unit identity for display |
| `on_discover` (dynamic) | AccommodationRate | Live price, availability, indicative cancellation windows |
| `on_select` | AccommodationRate | Selected rate echoed back |
| `on_init` | CancellationTerms | Binding terms, price lock, booking gate |
| `confirm` | GuestManifest | Guest PII sent by CN under consent |
| `on_confirm` | AccommodationReservation | Booking record created by supply system |
| `on_status` | AccommodationReservation | Status update; `cancellationPolicyText` added |
| `on_cancel` (try) | CancellationTerms | Cancel charge quote returned |
| `on_cancel` (commit) | AccommodationReservation | `cancellationDate`, `cancellationChargeAmount`, `refundAmount` populated |

## Design Decisions

1. **Shared `offerAttributes` container:** `AccommodationRate` and `CancellationTerms` both attach to `offerAttributes` but at different stages. AccommodationRate carries indicative (non-binding) data from the supply system's availability response. CancellationTerms carries binding data confirmed through a separate pre-commit API call at `init`. These are intentionally separate schemas — merging them would conflate indicative and binding data.

2. **Indicative vs binding cancellation windows:** `AccommodationRate.indicativeCancellationWindows` are for display only and must never be used for actual charge calculations. The binding source of truth is `CancellationTerms.bindingCancellationWindows` (Format 2) or `CancellationTerms.cancellationHours` + `appliedChargeAmount` (Format 1), confirmed at `init`.

3. **Session expiry enforcement:** `AccommodationRate.sessionExpiresAt` reflects the 20-minute search session window enforced by supply systems. The CN network must complete `select`, `init`, and `confirm` before this timestamp. Expired sessions require a fresh `discover`.

4. **Denormalised property identity in AccommodationResource:** Property-level fields (`propertyName`, `starRating`, `propertyAddress`, `propertyAmenities`, etc.) are repeated in every unit resource rather than held in a separate property entity. This makes each resource self-contained for discovery and avoids requiring a join at the CN layer.

5. **PII containment in GuestManifest:** All guest name and identity fields are confined to `GuestManifest` in `commitmentAttributes`. No PII appears in `resourceAttributes` or `offerAttributes`. The schema must not be returned in `on_discover` or `on_select` responses, and every payload must carry a `consentArtifact` per the ONT consent framework.

6. **Two supply system response formats in CancellationTerms:** The schema normalises two formats returned by different supply APIs — Format 1 (a single `cancellationHours` + `appliedChargeAmount`) and Format 2 (a list of `bindingCancellationWindows` with start/end datetimes and charge amounts). The CN must handle both without assuming which format a given supply system returns.

## Standards Compliance

- **Format:** OpenAPI 3.1.1
- **Semantics:** RDF/RDFS vocabulary
- **Linked Data:** JSON-LD context
- **Protocol:** Beckn v2.0
- **Dates/Times:** ISO 8601
- **Currency:** ISO 4217 codes
- **Meal Basis:** Industry standard codes (RO, BB, HB, FB, AI)
