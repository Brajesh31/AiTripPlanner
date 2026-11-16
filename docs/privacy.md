# Privacy and User Data

## Data handled
The code indicates that the app handles Google profile information, email address and profile picture in local storage, destination, trip duration, budget, traveler group, generated hotel and itinerary details, and saved Firestore trip records. Travel plans can be sensitive because a destination and date may reveal where a person intends to be and when.

## Local storage
The current code stores the Google user-info response under local-storage key `user`. Local storage persists in the browser and is accessible to JavaScript on the same origin. Do not treat it as a secure session or trusted identity. Avoid retaining unnecessary profile fields, do not log the profile object, and clear only application-owned values on logout.

## Firestore records
Trip records in `AITrips` include a profile email, user selections, generated trip data, and an ID. The client-side query filters by email but does not provide access control. Firestore Security Rules must enforce ownership for every read and write. No Firestore rules file was present in the inspected repository.

## External providers
Trip preferences are sent to Gemini. Google OAuth is used to retrieve profile details. Places autocomplete may communicate entered terms with Google services. Firebase receives saved trip records. Before production, map the fields sent to each provider, their purpose, provider terms, configuration, and user disclosures.

## Data minimization
Collect only what is required to generate and save a trip. Avoid exact home addresses, passport details, payment information, identity documents, or sensitive personal information. Do not add analytics or tracking without a documented purpose and privacy review.

## Retention and deletion
The current source does not provide a complete data-retention or account-deletion workflow. Define how users delete individual trips, delete all saved records, remove browser profile state, and request deletion from backups/logs. Explain retention periods and deletion behavior to users.

## Development and issue reporting
Use synthetic profiles and destinations in development. Never paste private itineraries, access tokens, real account profile data, or identifiable screenshots into public issues.

## Checklist
- [ ] User data and provider flows are inventoried.
- [ ] Firestore ownership rules are enforced and tested.
- [ ] Local storage contains only necessary profile state.
- [ ] Tokens and sensitive itinerary data are excluded from logs.
- [ ] Privacy notice accurately describes provider usage.
- [ ] Retention and deletion workflows are defined.
- [ ] Development fixtures contain synthetic data.
- [ ] Analytics and third-party scripts are reviewed.

See [Authentication](authentication.md), [Database](database.md), and [Security](security.md).
