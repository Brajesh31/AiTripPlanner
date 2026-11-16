# Application Architecture

## Overview
AI Trip Planner is a React single-page application bootstrapped by Vite. React renders the interface, React Router maps browser paths to views, and service modules integrate with Google and Firebase.

The current design is browser-heavy. Google OAuth and the Google user-info request run in the client, Gemini is called through a browser-side SDK, and Firestore reads and writes use the Firebase client SDK. These boundaries matter for security and reliability.

## High-level diagram
```text
Browser
  |
  +-- Vite-built React application
  |     +-- Header: Google OAuth and local-storage profile
  |     +-- React Router
  |     |     +-- /                -> Hero
  |     |     +-- /create-trip    -> CreateTrip
  |     |     +-- /my-trips       -> MyTrips
  |     |     +-- /view-trip/:id  -> Trip detail
  |     +-- Service modules
  |           +-- AIModal -> Gemini SDK
  |           +-- FirebaseConfig -> Firestore
  |           +-- GlobalApi -> commented Places REST helper
  |
  +-- Google OAuth / User Info endpoint
  +-- Gemini API
  +-- Google Places autocomplete integration
  +-- Firebase Cloud Firestore
```

## Startup and routing
`src/main.jsx` creates the browser router, mounts the Google OAuth provider, renders the global header, route provider, and toast container. Routes are `/`, `/create-trip`, `/my-trips`, and `/view-trip/:tripId`. `src/App.jsx` renders the landing hero. `Header.jsx` is mounted globally outside route elements.

## Feature modules
- **Landing and header:** `Hero.jsx` introduces the application. `Header.jsx` reads the profile from browser local storage and renders sign-in or user actions.
- **Trip creation:** `src/create-trip/index.jsx` collects destination, duration, budget, and traveler group. It prompts Google sign-in when a local profile is missing, builds a prompt from `src/constants/Options.jsx`, and sends it to Gemini.
- **AI service:** `src/service/AIModal.jsx` initializes the Google Generative AI SDK using `VITE_GOOGLE_GEMINI_AI_API_KEY`, configures a Gemini model, and starts a chat session with seeded example messages.
- **Firebase service:** `src/service/FirebaseConfig.jsx` initializes Firebase and exports Firestore. Trip creation writes an `AITrips` document.
- **Saved trips:** `src/my-trips/index.jsx` queries records by the email stored in local storage.
- **Trip details:** `src/view-trip/[tripId]/Index.jsx` reads a document and passes data to overview, hotels, places, and footer components.

## Current data flow
1. Visitor opens the landing page and selects trip creation.
2. The form captures destination, duration, budget, and traveler group.
3. If no local profile exists, Google sign-in is requested.
4. The browser requests Google profile information and stores it in local storage.
5. The client composes a Gemini prompt and receives response text.
6. The client attempts to parse the text as JSON.
7. A Firestore document is written with selections, trip data, email, and ID.
8. The app navigates to the trip detail route.
9. My Trips queries saved documents using the locally stored email.

## Trust boundaries
The browser is controlled by the user. Local storage can be changed. A model response is untrusted. A URL parameter can be modified. A Firestore document ID can be supplied manually. Therefore local-storage email is not proof of identity, client-side filtering is not authorization, model output must be validated, and Firestore Security Rules must enforce ownership.

## Current data expectations
A saved record contains `id`, `userEmail`, `userSelection`, and `tripData`. The UI expects `tripData.hotel_options` and `tripData.itinerary`, with fields such as `HotelName`, `HotelAddress`, `Price`, `Rating`, `best_time_to_visit`, and `places`. These are observed client expectations, not a formal schema guarantee.

## Recommended production architecture
Move Gemini and privileged Places requests behind server-side endpoints. Verify user identity on trusted infrastructure and enforce ownership with Firestore rules. Validate AI output against a runtime schema. Add explicit provider error handling, rate limiting, audit-safe logging, and integration tests. These are recommendations, not implemented features.

See [API](api.md), [Authentication](authentication.md), [AI Generation](ai-generation.md), [Data Model](data-model.md), [Database](database.md), and [Security](security.md).
