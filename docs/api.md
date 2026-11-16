# API and External Service Integrations

## 1. First-party API status

The repository does not implement its own backend API server or a documented set of first-party REST endpoints. It does integrate browser-side SDKs and provider endpoints. This distinction matters: provider SDK calls are not the same as a secure server-side API owned by this application.

## 2. Current integrations

| Integration | Current source | Observed usage |
|---|---|---|
| Gemini | `src/service/AIModal.jsx` | Browser SDK creates a chat session and generates itinerary text |
| Google OAuth | `src/main.jsx`, `Header.jsx`, `create-trip/index.jsx` | Browser OAuth sign-in |
| Google User Info | `create-trip/index.jsx`, `Header.jsx` | Axios GET with OAuth access token |
| Firebase Firestore | `FirebaseConfig.jsx`, trip pages | Browser SDK reads and writes `AITrips` documents |
| Google Places | `create-trip/index.jsx`, `GlobalApi.jsx` | Autocomplete package imported; current form uses text input and helper is commented out |

## 3. Gemini request

The AI service reads `VITE_GOOGLE_GEMINI_AI_API_KEY`, creates a GoogleGenerativeAI instance, selects `gemini-1.5-flash`, and configures JSON MIME output with temperature, topP, topK, and max output tokens.

The trip form substitutes destination, day count, traveler category, and budget into `AI_PROMPT`, then calls `chatSession.sendMessage(Final_Prompt)`.

Expected output is a JSON object describing `hotel_options` and an `itinerary` grouped by day. The UI consumes fields such as `HotelName`, `HotelAddress`, `Price`, `Rating`, `best_time_to_visit`, and `places`.

The current implementation parses raw model text using JSON.parse. It does not strip Markdown fences or validate a schema before storing. Model output must be treated as untrusted, probabilistic data. See [AI Output Contract](ai-output-contract.md).

## 4. Google OAuth and user profile request

The application uses `@react-oauth/google` with the client ID from `VITE_GOOGLE_AUTH_CLIENT_ID`. On success, it extracts an access token and requests:

```text
GET https://www.googleapis.com/oauth2/v1/userinfo
Authorization: Bearer <OAuth access token>
Accept: application/json
```

The profile response is stored in localStorage under the key `user`. Do not log or share full profile responses or access tokens. The local cache is not a trusted authentication mechanism for database authorization.

## 5. Firestore operations

The client code performs:
- `setDoc` to create a trip document in `AITrips`;
- `getDocs` on a query filtered by `userEmail`;
- `getDoc` for a document ID taken from the route parameter.

These operations use the Firebase Web SDK. Firestore Security Rules must enforce authentication and ownership. See [Database](database.md) and [Firestore Data Model](firestore-data-model.md).

## 6. Google Places status

The form imports `react-google-places-autocomplete`, but the current location field is an ordinary text input. `src/service/GlobalApi.jsx` contains an inactive, commented-out request helper. A Places API key appears in `.env.example`, but its current active use is not demonstrated by the inspected helper.

Do not assume autocomplete suggestions or place details are currently fetched.

## 7. Error and response handling

Current async paths do not consistently handle:
- provider timeouts or network failures;
- HTTP authentication errors;
- quota and rate-limit responses;
- malformed or incomplete model JSON;
- Firestore permission errors;
- documents with missing nested fields;
- expired OAuth access tokens.

Add explicit loading, success, and failure states before describing integrations as production-ready.

## 8. Requirements before adding endpoints

If a backend API is added, document purpose, route, method, auth model, validation, request and response schema, error codes, quota, retry behavior, logging, privacy, retention, secret storage, monitoring, and versioning. Never expose a privileged provider key in browser code.

## 9. Provider documentation

Provider behavior and API versions can change. Consult the current official Google and Firebase documentation before changing SDK usage or deployment configuration. Record the date and version used when a compatibility decision matters.
