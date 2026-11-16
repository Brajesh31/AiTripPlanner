# Database and Firestore

## Technology and current use
The app uses Firebase Cloud Firestore through the Firebase JavaScript SDK. Initialization is in `src/service/FirebaseConfig.jsx`. Client code reads and writes a collection named `AITrips`.

This describes fields expected by current components. It is not a guarantee that every generated record conforms to a validated schema.

## AITrips collection
The create-trip view writes fields:

| Field | Expected type | Description |
|---|---|---|
| `id` | string | Application trip identifier |
| `userEmail` | string | Email copied from the stored Google profile |
| `userSelection` | object | User-provided preferences |
| `tripData` | object | Parsed AI-generated itinerary |

The document ID is generated client-side from the current timestamp string. This may collide under concurrent operations. Prefer Firestore-generated IDs or a collision-resistant approach.

## UserSelection
Current form values include `location`, `nOofdays`, `budget`, and `traveler`. Field spelling and case are significant to the current client. Renaming fields requires updating readers, writers, and potentially migrating existing records.

## TripData
The UI expects `hotel_options` and `itinerary`. Each itinerary day should contain `best_time_to_visit` and a `places` array. Hotel properties include `HotelName`, `HotelAddress`, `Price`, and `Rating`. Place properties include name, description, image URL, coordinates, ticket pricing, and travel time.

Formalize this contract with a runtime schema. A model prompt does not guarantee exact output.

## Current read patterns
My Trips queries `AITrips` where `userEmail` equals the email from local storage. The detail screen reads a document by route ID. The email filter is not authorization. Local storage is editable and document IDs can be supplied manually. Security Rules must validate authenticated ownership.

## Firestore Security Rules
No Firestore rules file was present in the inspected repository tree. Do not assume production data is protected. Do not deploy unrestricted public read/write rules. Require authenticated identities and enforce document ownership. Test that users can read their own trips but cannot read or modify other users' documents. Consider using Firebase Authentication as the identity source rather than relying on a local profile object.

## Validation before persistence
Before a model response is stored:
1. parse the response safely;
2. validate the top-level object;
3. validate the hotel array and each hotel;
4. validate itinerary day entries and place arrays;
5. enforce reasonable string lengths and numeric bounds;
6. reject malformed URLs;
7. enforce output-size limits;
8. handle empty and partial sections;
9. show an actionable error without logging private content.

## Indexes and query performance
The current query filters by `userEmail`. More complex future queries may require Firestore composite indexes. Create indexes based on actual queries and Firebase guidance.

## Retention and deletion
The current UI does not provide a complete account or trip deletion workflow. Before production, define deletion of individual trips and all associated records, retention, and backup behavior.

## Schema migrations
When persisted fields change, document old and new shapes, decide whether migration is required, update writers and readers, handle missing fields during transition, validate production data, and retain a rollback plan.

See [Data Model](data-model.md), [Authentication](authentication.md), [Privacy](privacy.md), and [Security](security.md).
