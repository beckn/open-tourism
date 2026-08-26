# AccommodationResource — v1.0

Static catalog attributes for an accommodation unit type at a property. Published via `catalog/publish` on a cron schedule and returned in the static `on_discover` response. Carries no live availability, no pricing, and no PII. Property-level context is denormalized into every unit resource so each is self-contained for discovery.

Beckn container: `resourceAttributes`

## Files

| File | Purpose |
|---|---|
| [https://schema.beckn.io/AccommodationResource/attributes.yaml](https://schema.beckn.io/AccommodationResource/attributes.yaml) | OpenAPI schema envelope (latest path) |
| [https://schema.beckn.io/AccommodationResource/v1.0/attributes.yaml](https://schema.beckn.io/AccommodationResource/v1.0/attributes.yaml) | OpenAPI schema envelope (versioned path) |
| [https://schema.beckn.io/AccommodationResource/context.jsonld](https://schema.beckn.io/AccommodationResource/context.jsonld) | JSON-LD context (latest path) |
| [https://schema.beckn.io/AccommodationResource/v1.0/context.jsonld](https://schema.beckn.io/AccommodationResource/v1.0/context.jsonld) | JSON-LD context (versioned path) |
| [https://schema.beckn.io/AccommodationResource/vocab.jsonld](https://schema.beckn.io/AccommodationResource/vocab.jsonld) | RDF vocabulary (latest path) |
| [https://schema.beckn.io/AccommodationResource/v1.0/vocab.jsonld](https://schema.beckn.io/AccommodationResource/v1.0/vocab.jsonld) | RDF vocabulary (versioned path) |
| [https://schema.beckn.io/AccommodationResource/profile.json](https://schema.beckn.io/AccommodationResource/profile.json) | Beckn profile descriptor (latest path) |
| [https://schema.beckn.io/AccommodationResource/v1.0/profile.json](https://schema.beckn.io/AccommodationResource/v1.0/profile.json) | Beckn profile descriptor (versioned path) |

## Properties

| Property | Required | Type | Description |
|---|---|---|---|
| `localPropertyId` | no | string | Stable supply-system identifier for the property. Persists across sessions; join key between static catalog and live availability. |
| `propertyName` | no | string | Display name of the accommodation property. |
| `starRating` | no | number | Official star classification on the 1.0–5.0 scale, as assigned by the supply system. |
| `propertyAddress` | no | string | Full postal address of the property. |
| `propertyDescription` | no | string | Narrative description of the property. May be empty for some supply system records. |
| `propertyImages` | no | array of object | Collection of property images. Each entry carries `thumbnailUrl` (list views) and `fullUrl` (detail views). |
| `propertyAmenities` | no | array of string | Property-level amenity names available to all guests (e.g. Swimming Pool, Gym). |
| `unitAmenities` | no | array of string | Unit-level amenity names present in the accommodation unit (e.g. Air Conditioning, Safe). |
| `contactPhone` | no | string | Property telephone number. Conveyed only where fulfilment requires direct property contact. |
| `contactEmail` | no | string | Property email address. Conveyed only where fulfilment requires it. |
| `contactWebsite` | no | string | Property website URL. |
