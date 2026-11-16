# API and External Service Integrations

## Current API boundary
The repository does not implement its own backend API server or a documented first-party REST endpoint set. It does integrate browser-side SDKs and provider endpoints.

Observed status:
- Gemini SDK: active in `src/service/AIModal.jsx`.
- Google OAuth: active through `@react-oauth/google`.
- Google User Info endpoint: called with the OAuth access token.
- Firebase Firestore: browser SDK reads and writes records.
- Places autocomplete: imported in the trip creation view.
- Places REST helper: commented out in `src/service/GlobalApi.jsx`, therefore inactive.

## Gemini
The app imports `GoogleGenerativeAI`, reads `VITE_GOOGLE_GEMINI_AI_API_KEY`, configures a Gemini model, starts a chat session, and sends a prompt generated from trip preferences. The requested response is JSON containing hotel options and a day-by-day itinerary. The client consumes response text and attempts JSON parsing.

Limitations: output is not trusted API data; JSON may be malformed or use unexpected field names; hotel and place claims may be fabricated or stale; a browser-exposed key can be extracted; quotas, model names, and SDK behavior can change.

For production, proxy requests through a server endpoint, store the key server-side, enforce quotas, validate requests and responses, and handle rate limits and outages.

## Google OAuth and user profile
The app uses the Google OAuth React integration. After login, the browser requests user information using the access token. The response is stored in local storage under `user`. This is a client-side profile convenience, not a complete secure application session system. Do not treat a local-storage email as proof of authorization. A trusted server must validate identity for privileged operations.

## Firebase
Firebase initializes in `src/service/FirebaseConfig.jsx`. The trip-creation flow writes to `AITrips`; My Trips queries by email; trip details read by document ID. This is a Firestore SDK integration rather than a first-party REST contract. Security Rules must enforce authenticated ownership.

## Google Places
The trip form imports `react-google-places-autocomplete`. The helper in `GlobalApi.jsx` is commented out and is not an active request path. Configure required Google Maps Platform APIs, billing, allowed origins, and key restrictions in Google Cloud.

## Future first-party endpoints
If a backend is added, document method, path, purpose, authentication, authorization, request/response schema, validation, status codes, rate limits, retry policy, logging, retention, and versioning for every endpoint.

## Provider failures
Distinguish invalid credentials, permission errors, quota exhaustion, rate limits, network failures, timeouts, malformed responses, and outages. User-facing messages must not disclose credentials, tokens, private prompts, or personal data.

See [AI Generation](ai-generation.md), [Authentication](authentication.md), [Environment Configuration](environment-configuration.md), and [Security](security.md).
