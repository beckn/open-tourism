# Open Tourism Network v1.0 Schema Pack Index

Complete index of the ONT v1.0 schema pack for hotel and accommodation booking on the Beckn Protocol.

---

## Quick Navigation

### Schema 1: AccommodationResource
- **Purpose:** Static catalog attributes for a bookable accommodation unit type at a property
- **Container:** `resourceAttributes`
- **Prefix:** `ar` → https://schema.beckn.io/AccommodationResource#
- **Files:**
  - [attributes.yaml](./AccommodationResource/v1.0/attributes.yaml) - OpenAPI 3.1.1 schema definition
  - [context.jsonld](./AccommodationResource/v1.0/context.jsonld) - JSON-LD namespace context
  - [vocab.jsonld](./AccommodationResource/v1.0/vocab.jsonld) - RDF vocabulary definitions
  - [profile.json](./AccommodationResource/v1.0/profile.json) - Beckn protocol profile
  - [README.md](./AccommodationResource/README.md) - Full documentation

**Key Properties:**
- `localPropertyId`: Stable supply-system property identifier — join key between static catalog and live availability
- `propertyName`, `starRating`, `propertyAddress`, `propertyDescription`: Core property identity fields
- `propertyImages`: Thumbnail and full-size image pairs for list and detail views
- `propertyAmenities`: Property-level amenities available to all guests
- `unitAmenities`: Unit-level amenities present in the accommodation unit itself
- `contactPhone`, `contactEmail`, `contactWebsite`: Property contact details

---

### Schema 2: AccommodationRate
- **Purpose:** Live pricing and availability for a specific room-rate combination within a search session
- **Container:** `offerAttributes`
- **Prefix:** `acr` → https://schema.beckn.io/AccommodationRate#
- **Files:**
  - [attributes.yaml](./AccommodationRate/v1.0/attributes.yaml) - OpenAPI 3.1.1 schema definition
  - [context.jsonld](./AccommodationRate/v1.0/context.jsonld) - JSON-LD namespace context
  - [vocab.jsonld](./AccommodationRate/v1.0/vocab.jsonld) - RDF vocabulary definitions
  - [profile.json](./AccommodationRate/v1.0/profile.json) - Beckn protocol profile
  - [README.md](./AccommodationRate/README.md) - Full documentation

**Key Properties:**
- `unitCategory`, `unitTypeDescription`: Room type and rate combination label
- `mealBasisCode`, `mealBasisLabel`: Meal plan (RO, BB, HB, FB, AI)
- `totalRateAmount`, `rateCurrency`: Total price for the full stay
- `nightlyRates`: Per-night rate breakdown with date, day-of-week, and nightly amount
- `refundable`: Indicative refundability flag — binding terms confirmed at `init`
- `adultsCount`, `childrenCount`, `childAges`: Occupancy details
- `indicativeCancellationWindows`: Display-only cancellation windows — never use for actual charges
- `sessionExpiresAt`: 20-minute search session expiry — CN must complete the flow before this time

---

### Schema 3: CancellationTerms
- **Purpose:** Binding cancellation and refund terms confirmed at the pre-commit check, plus booking gate signals and cancel-quote results
- **Container:** `offerAttributes`
- **Prefix:** `ct` → https://schema.beckn.io/CancellationTerms#
- **Files:**
  - [attributes.yaml](./CancellationTerms/v1.0/attributes.yaml) - OpenAPI 3.1.1 schema definition
  - [context.jsonld](./CancellationTerms/v1.0/context.jsonld) - JSON-LD namespace context
  - [vocab.jsonld](./CancellationTerms/v1.0/vocab.jsonld) - RDF vocabulary definitions
  - [profile.json](./CancellationTerms/v1.0/profile.json) - Beckn protocol profile
  - [README.md](./CancellationTerms/README.md) - Full documentation

**Key Properties:**
- `bookingPermitted`: Booking gate — CN must disable confirm when `no`
- `unitSoldOut`: Availability gate — sold out since discovery
- `priceChanged`, `priceMovementAmount`: Price integrity signals — must be surfaced to the traveller
- `verifiedTotalAmount`, `verifiedTotalCurrency`: Price locked at pre-commit; becomes `expected_price` at confirm
- `cancellationHours`, `appliedChargeAmount`: Format 1 (hours-based) cancellation policy
- `bindingCancellationWindows`: Format 2 (datetime-window) ordered binding charge windows
- `cancellationChargeQuote`: Point-in-time cancel charge returned in `on_cancel` (try mode)

---

### Schema 4: AccommodationReservation
- **Purpose:** Booking lifecycle record returned from `on_confirm` and updated via `on_status` and `on_cancel`
- **Container:** `contractAttributes`
- **Prefix:** `ares` → https://schema.beckn.io/AccommodationReservation#
- **Files:**
  - [attributes.yaml](./AccommodationReservation/v1.0/attributes.yaml) - OpenAPI 3.1.1 schema definition
  - [context.jsonld](./AccommodationReservation/v1.0/context.jsonld) - JSON-LD namespace context
  - [vocab.jsonld](./AccommodationReservation/v1.0/vocab.jsonld) - RDF vocabulary definitions
  - [profile.json](./AccommodationReservation/v1.0/profile.json) - Beckn protocol profile
  - [README.md](./AccommodationReservation/README.md) - Full documentation

**Key Properties:**
- `bookingReference`, `agentReference`, `networkConfirmationRef`, `propertyConfirmationRef`: Reference identifier chain
- `reservationStatus`: Lifecycle status (`vouchered`, `on-request`, `failed`, `cancelled`, `rejected`)
- `totalChargeAmount`, `chargeCurrency`: Committed amount — must match `expected_price` sent at confirm
- `cancellationDeadline`: Must be shown on the booking confirmation
- `leadSalutation`, `leadFirstName`, `leadLastName`: Lead guest identity
- `checkInDate`, `checkOutDate`, `selectedNights`, `totalRooms`: Stay summary
- `roomDetail`: Per-room booking summary with type and count
- `cancellationDate`, `cancellationChargeAmount`, `refundAmount`: Populated in `on_cancel`

---

### Schema 5: GuestManifest
- **Purpose:** Per-room guest identity sent at `confirm` and echoed in `on_confirm`, under purpose-bound consent
- **Container:** `commitmentAttributes`
- **Prefix:** `gm` → https://schema.beckn.io/GuestManifest#
- **Files:**
  - [attributes.yaml](./GuestManifest/v1.0/attributes.yaml) - OpenAPI 3.1.1 schema definition
  - [context.jsonld](./GuestManifest/v1.0/context.jsonld) - JSON-LD namespace context
  - [vocab.jsonld](./GuestManifest/v1.0/vocab.jsonld) - RDF vocabulary definitions
  - [profile.json](./GuestManifest/v1.0/profile.json) - Beckn protocol profile
  - [README.md](./GuestManifest/README.md) - Full documentation

**Key Properties:**
- `passengers`: Ordered guest list — first entry is the lead guest; each entry carries salutation, name, type, and age
- `specialRequest`: Free-text guest request — conveyed to property, never guaranteed
- `consentArtifact`: Purpose-bound consent record required by ONT consent framework

---

## Technical Reference

### File Standards
Each schema pack contains exactly 5 files:

| File | Purpose | Format |
|------|---------|--------|
| `attributes.yaml` | Complete schema definition | OpenAPI 3.1.1 |
| `context.jsonld` | Namespace and prefix mappings | JSON-LD Context |
| `vocab.jsonld` | Semantic vocabulary definitions | RDF/RDFS |
| `profile.json` | Beckn protocol configuration | JSON |
| `README.md` | Full documentation | Markdown |

### Naming Conventions
- **Container Names:** camelCase (resourceAttributes, offerAttributes, contractAttributes, commitmentAttributes)
- **Properties:** camelCase (localPropertyId, totalRateAmount)
- **Classes:** PascalCase (AccommodationResource, AccommodationRate)
- **Prefixes:** Short lowercase with schema abbreviation (ar, acr, ares, ct, gm)

### Shared Container Note
`AccommodationRate` and `CancellationTerms` both attach to `offerAttributes`. They operate at different Beckn flow stages: AccommodationRate in `on_discover` (dynamic), CancellationTerms in `on_init` and `on_cancel`.

### Semantic Integration
All schemas import core Beckn vocabulary:
- **Vocabulary import:** `https://schema.beckn.io/core/v2/vocab.jsonld`
- **Root context:** `ONT/schema/context.jsonld`
- **Root vocab:** `ONT/schema/vocab.jsonld`

### Profile Configuration
All profiles specify:
- `protocol_version: "2.0"`
- `schema_version: "1.0.0"`

---

## Directory Structure

```
schema/
├── INDEX.md              (this file)
├── README-v1.0.md        (pack-level overview)
├── context.jsonld        (aggregate JSON-LD context)
├── vocab.jsonld          (aggregate RDF vocabulary)
├── AccommodationResource/
│   ├── README.md
│   └── v1.0/
│       ├── attributes.yaml
│       ├── context.jsonld
│       ├── vocab.jsonld
│       ├── profile.json
│       └── README.md
├── AccommodationRate/
│   ├── README.md
│   └── v1.0/
│       └── ...
├── CancellationTerms/
│   ├── README.md
│   └── v1.0/
│       └── ...
├── AccommodationReservation/
│   ├── README.md
│   └── v1.0/
│       └── ...
└── GuestManifest/
    ├── README.md
    └── v1.0/
        └── ...
```

---

## Validation Checklist

Use this checklist when integrating schemas into your system:

- [ ] All YAML files parse as valid OpenAPI 3.1.1
- [ ] All JSON files are valid JSON (syntax)
- [ ] All JSON-LD files are valid RDF/RDFS
- [ ] All `$ref` paths resolve correctly
- [ ] All `x-jsonld` annotations use valid URIs
- [ ] Root `context.jsonld` and `vocab.jsonld` present at `schema/` level
- [ ] Each schema has `context.jsonld`, `vocab.jsonld`, `attributes.yaml`, `profile.json`, `README.md`
- [ ] `bookingPermitted: no` blocks confirm flow in CN implementation
- [ ] `sessionExpiresAt` enforced — CN must not proceed after expiry
- [ ] `GuestManifest` never returned in `on_discover` or `on_select`
- [ ] `consentArtifact` present in every `GuestManifest` payload

---

**Version:** 1.0.0
**Domain:** Open Tourism Network — Hotel and Accommodation Booking
**Protocol:** Beckn v2.0
**Total Schemas:** 5
