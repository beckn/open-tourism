# GuestManifest — v1.0

Per-room guest identity details sent at the `confirm` stage and echoed in `on_confirm`. Contains PII and must not appear in `on_discover` or `on_select` responses. Shared only with the fulfilling property and its supply system under explicit, purpose-bound consent. The lead guest is the first passenger entry for each room. Child ages are mandatory for every child passenger — they determine rate eligibility and property occupancy rules.

Beckn container: `commitmentAttributes`

## Files

| File | Purpose |
|---|---|
| [https://schema.beckn.io/GuestManifest/attributes.yaml](https://schema.beckn.io/GuestManifest/attributes.yaml) | OpenAPI schema envelope (latest path) |
| [https://schema.beckn.io/GuestManifest/v1.0/attributes.yaml](https://schema.beckn.io/GuestManifest/v1.0/attributes.yaml) | OpenAPI schema envelope (versioned path) |
| [https://schema.beckn.io/GuestManifest/context.jsonld](https://schema.beckn.io/GuestManifest/context.jsonld) | JSON-LD context (latest path) |
| [https://schema.beckn.io/GuestManifest/v1.0/context.jsonld](https://schema.beckn.io/GuestManifest/v1.0/context.jsonld) | JSON-LD context (versioned path) |
| [https://schema.beckn.io/GuestManifest/vocab.jsonld](https://schema.beckn.io/GuestManifest/vocab.jsonld) | RDF vocabulary (latest path) |
| [https://schema.beckn.io/GuestManifest/v1.0/vocab.jsonld](https://schema.beckn.io/GuestManifest/v1.0/vocab.jsonld) | RDF vocabulary (versioned path) |
| [https://schema.beckn.io/GuestManifest/profile.json](https://schema.beckn.io/GuestManifest/profile.json) | Beckn profile descriptor (latest path) |
| [https://schema.beckn.io/GuestManifest/v1.0/profile.json](https://schema.beckn.io/GuestManifest/v1.0/profile.json) | Beckn profile descriptor (versioned path) |

## Properties

| Property | Required | Type | Description |
|---|---|---|---|
| `passengers` | no | array of object | Ordered list of guests for this room. First entry is the lead guest. Each entry carries `salutation`, `firstName`, `lastName`, `passengerType` (`adult` or `child`), and `age` (required for children). |
| `specialRequest` | no | string | Free-text special request for the room (e.g. non-smoking, late check-in). Conveyed to the property but never guaranteed. |
| `specialRequestDisclaimer` | no | string | Standard disclaimer echoed by the supply system that special requests are not guaranteed and subject to availability. |
| `consentArtifact` | no | object | Purpose-bound consent record authorising transmission of guest PII to the fulfilling property. Required by the ONT consent framework. Carries `consentId`, `subject`, `purpose`, `scope`, `grantedAt`, `expiresAt`, `mechanism`, and `mediator`. |
