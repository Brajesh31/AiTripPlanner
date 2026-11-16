# Firestore Data Model and Access Expectations

## Collection
The app uses the Firestore collection `AITrips`. Trip creation writes a document, My Trips queries documents, and the detail route reads by document ID.

This page describes the current client data shape, not a validated production schema.

## Current record shape
```json
{
  "userSelection": {
    "location": "Example destination",
    "nOofdays": 4,
    "budget": "Moderate",
    "traveler": "A Couple"
  },
  "tripData": {
    "hotel_options": [],
    "itinerary": {}
  },
  "userEmail": "synthetic-user@example.invalid",
  "id": "document-id"
}
```

The example is synthetic. Current code creates the ID with `Date.now().toString()` and stores it both as document path and in the `id` field. Prefer a Firestore-generated ID in a future implementation. Do not rely on IDs being secret.

## Current operations
- **Create:** `src/create-trip/index.jsx` calls `setDoc` on `doc(db, 'AITrips', docId)`.
- **List:** `src/my-trips/index.jsx` queries `AITrips` where `userEmail` matches the email in localStorage.
- **Detail:** `src/view-trip/[tripId]/Index.jsx` reads the document using the route parameter.

## Field expectations
| Field | Purpose | Risk |
|---|---|---|
| `userSelection` | User's destination, duration, budget, and traveler selection | Duration key spelling is inconsistent in source |
| `tripData` | Parsed Gemini output | No complete schema validation |
| `userEmail` | My Trips query filter | Not a secure ownership boundary |
| `id` | Copy of document identifier | Redundant and timestamp-generated |

## Rules are mandatory
The email query is not authorization. Visitors can modify browser storage and call Firestore directly. Rules must independently require trusted authentication and restrict create/read/update/delete to the record owner. Owner identity must be set from trusted auth context and be immutable. Validate field types and document size.

The inspected source does not include a Firestore rules file or emulator test configuration. Treat secure rules as an unresolved deployment prerequisite. Do not deploy real user itineraries until rules are created and tested.

## Authentication alignment
The project uses Google OAuth profile retrieval separately from Firebase. A secure rule set cannot trust the locally cached Google profile. Align the app with Firebase Authentication or a trusted backend before enforcing ownership through authenticated UID.

## Data validation
Validate Gemini output before writing and defensively validate saved records before rendering. Verify root shape, hotel array, itinerary object, day records, and places arrays. Constrain strings, counts, and data size. Consider `schemaVersion` if records with different response contracts will coexist.

## Deletion and retention
The current UI does not provide a documented trip deletion or account deletion process. Define trip deletion, account closure behavior, backup retention, and user data export before production use.

## Operations
Use separate Firebase projects for development and production. Restrict console access, review quotas, document backup/restore ownership, and never commit service account JSON or private credentials.

## Rule test strategy
Use Firestore Emulator tests or a controlled non-production project. Test unauthenticated requests, user A attempting to read user B records, spoofed email, owner mutation, oversized fields, and normal create/list/detail behavior. Keep test fixtures synthetic.
