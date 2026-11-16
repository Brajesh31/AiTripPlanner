# Trip Data Model

## Purpose

This document describes the data shape expected by the current React components and Firestore interactions. It is an observed client contract, not a formal schema guarantee. Gemini output can deviate from the expected fields, types, or casing.

## Firestore document

Trip documents are written to the `AITrips` collection. Current code stores:

| Field | Purpose | Expected representation |
|---|---|---|
| `id` | Application-level identifier | String |
| `userEmail` | Profile email used by current My Trips query | String |
| `userSelection` | Original form selections | Object |
| `tripData` | Parsed model response | Object |

The current client uses a timestamp string for the document ID. This is not a robust uniqueness guarantee. Prefer a Firestore-generated ID or another collision-resistant identifier.

## User selection

| Property | Meaning |
|---|---|
| `location` | Requested destination |
| `nOofdays` | Trip duration |
| `budget` | Budget category |
| `traveler` | Traveler group label |

The source currently uses the unusual capitalization `nOofdays`. Do not rename it without updating all readers/writers and migrating existing Firestore records.

## Generated trip object

The current views expect these top-level properties:

| Property | Expected content |
|---|---|
| `trip_details` | Destination, duration, traveler, budget, if returned |
| `hotel_options` | Array of recommended hotel objects |
| `itinerary` | Object keyed by day |

A hotel object is expected to include properties such as `HotelName`, `HotelAddress`, `Price`, and `Rating`. The prompt also requests image URLs, geographic coordinates, and descriptions.

Each itinerary day is expected to contain:
- `best_time_to_visit`;
- `places`, an array of place objects.

Place objects are expected to provide a place name, details, image URL, coordinates, ticket pricing, and travel-time estimate. Verify actual field access in `PlaceCardItem.jsx` before changing this contract.

## Field casing

The codebase mixes camelCase top-level keys with PascalCase hotel/place keys. JSON property names are case-sensitive. Changing `HotelName` to `hotelName`, for example, will break current readers unless all dependent components are updated.

A future schema should use one consistent naming convention, ideally camelCase, and include an explicit version.

## Validation

Do not persist raw parsed model output directly. Validate:
- top-level object shape;
- required trip fields;
- hotel arrays and individual hotel fields;
- itinerary object and day entries;
- place arrays and individual place fields;
- strings, numbers, URLs, and coordinates;
- maximum array lengths and text sizes.

Normalize validated records into the application's canonical shape. Handle missing optional fields explicitly.

## Data ownership

A document must be owned by a verified authenticated identity. The current `userEmail` value comes from local storage and is not proof of ownership. Firestore rules should enforce ownership using the authenticated user's UID. Consider storing an immutable `userId` field instead of relying on email, which can change.

## Schema evolution

Add a `schemaVersion` field before making substantial changes. For migrations:
1. document current and target schema;
2. identify existing records requiring conversion;
3. update readers to tolerate the transition;
4. deploy writers and migration code in a controlled order;
5. validate representative records;
6. retain rollback and recovery procedures.

## Example shape

The following is a conceptual outline, not a guarantee that Gemini currently returns exactly this object:

```json
{
  "id": "firestore-document-id",
  "userId": "authenticated-user-id",
  "userSelection": {
    "location": "Sample destination",
    "nOofdays": 3,
    "budget": "Moderate",
    "traveler": "A Couple"
  },
  "tripData": {
    "hotel_options": [],
    "itinerary": {}
  },
  "schemaVersion": 1
}
```

Do not insert real traveler information into documentation examples.

See [Database](database.md), [AI Generation](ai-generation.md), [Authentication](authentication.md), and [Privacy](privacy.md).
