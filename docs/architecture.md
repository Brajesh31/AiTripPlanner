# Application Architecture

## 1. Overview

AI Trip Planner is a React single-page application bootstrapped by Vite. React renders the UI, React Router maps browser paths to views, Tailwind CSS provides styling utilities, and service modules integrate browser-side Google and Firebase SDKs.

The application is currently browser-heavy. OAuth, profile retrieval, Gemini generation, and Firestore operations are initiated from client code. There is no first-party backend API in this repository.

## 2. Runtime composition

```text
index.html
  |
  +-- Vite module entry: /src/main.jsx
        |
        +-- React StrictMode
        +-- GoogleOAuthProvider
        |     |
        |     +-- Header
        |     +-- RouterProvider
        |     |     +-- /                   -> App -> Hero
        |     |     +-- /create-trip        -> CreateTrip
        |     |     +-- /view-trip/:tripId  -> ViewTrip
        |     |     +-- /my-trips           -> MyTrips
        |     |
        |     +-- Toaster
        |
        +-- Service modules
              +-- AIModal.jsx       -> Gemini browser SDK
              +-- FirebaseConfig   -> Firebase app + Firestore
              +-- GlobalApi.jsx    -> commented Places request helper
```

## 3. Application entry and providers

`index.html` contains the root element and loads `src/main.jsx`. The entry module imports global CSS, creates the browser router, initializes the React root, and mounts the application under StrictMode and GoogleOAuthProvider.

The OAuth client ID is read from `VITE_GOOGLE_AUTH_CLIENT_ID`. The header is mounted outside RouterProvider, although it uses links for navigation. Toast notifications are made available through the Toaster component.

## 4. Route responsibilities

| Route | Source | Responsibility |
|---|---|---|
| `/` | `src/App.jsx` | Landing page and Hero |
| `/create-trip` | `src/create-trip/index.jsx` | Collect trip preferences, request AI output, and save a trip |
| `/view-trip/:tripId` | `src/view-trip/[tripId]/Index.jsx` | Load one Firestore document and render itinerary sections |
| `/my-trips` | `src/my-trips/index.jsx` | Query Firestore documents associated with cached user email |

The route parameter `tripId` is used as the Firestore document ID in the `AITrips` collection.

## 5. UI layers

### Custom components
- `Header.jsx`: navigation, sign-in dialog, profile menu, and logout presentation.
- `Hero.jsx`: landing-page headline and navigation to trip creation.
- `UserTripCardItem.jsx`: saved trip preview card.

### Trip creation
- `create-trip/index.jsx`: form state, Google sign-in, profile request, prompt substitution, Gemini call, JSON parsing, Firestore write, and route navigation.

### Trip details
- `InfoSection.jsx`: destination and preference summary.
- `Hotels.jsx`: hotel cards linking to Google Maps search URLs.
- `PlacesToVisit.jsx`: groups itinerary entries by day.
- `PlaceCardItem.jsx`: renders individual activity/place details.
- `Footer.jsx`: detail-page footer.

### UI primitives
The `src/components/ui/` directory contains button, dialog, input, popover, toast, toaster, and toast state helpers. Several use Radix UI primitives and class-variance-authority.

## 6. Service boundaries

### Gemini
`src/service/AIModal.jsx` instantiates the Google Generative AI SDK in the browser, configures the `gemini-1.5-flash` model, and exports a chat session with a large embedded sample conversation. The form sends a prompt built from destination, duration, traveler group, and budget.

This is a prototype integration, not a trusted server boundary. The browser-visible key can be extracted from the built assets. The hard-coded initial conversation is a large static example and may affect prompt behavior and bundle size.

### Firebase / Firestore
`src/service/FirebaseConfig.jsx` initializes a Firebase app and exports Firestore. Trip creation writes to `AITrips`; My Trips queries by `userEmail`; the detail view reads by document ID.

Client-side filtering is not authorization. Firestore rules must enforce user ownership on every read and write.

### Google OAuth
`@react-oauth/google` provides a browser OAuth flow. The client obtains an access token, then the application calls Google's User Info endpoint with Axios and stores the returned profile response in localStorage. This local profile cache is not a server-verified session and should not be used as an authorization authority.

### Google Places
The trip form imports `react-google-places-autocomplete`, but the current form uses a regular text input for the destination. `GlobalApi.jsx` contains a commented-out Places API request helper. Do not describe a working Google Places autocomplete flow as verified until the current form uses it.

## 7. Data flow

1. The visitor opens the landing route.
2. The visitor selects Create Trip.
3. The trip form collects destination, day count, budget, and traveler category.
4. If no cached profile exists, the app opens Google sign-in.
5. The access token is used to retrieve profile data.
6. Profile data is stored in localStorage.
7. The form substitutes values into the AI prompt.
8. The browser calls Gemini and receives text expected to contain JSON.
9. The response is parsed with JSON.parse.
10. A Firestore document is written with user selection, trip data, email, and ID.
11. The browser navigates to the trip detail route.
12. The detail view reads the document and renders hotels and day-by-day places.

Each external step can fail. The current code does not consistently handle rejected requests, malformed data, missing fields, unauthorized access, or rate limits.

## 8. Styling and build configuration

- Tailwind configuration is in `tailwind.config.js`.
- PostCSS configuration is in `postcss.config.js`.
- Alias `@/` is configured by Vite to refer to `src/`.
- `jsconfig.json` provides the editor path alias.
- `components.json` records component generator aliases.
- Vercel rewrite configuration sends application paths to `index.html`.

## 9. Known architecture risks

- Gemini is called from the browser with a Vite-exposed API key.
- Firebase config is hard-coded and needs rule/restriction review.
- LocalStorage profile data is trusted by UI code without a server session.
- Firestore reads by document ID must be authorized in rules, not by route secrecy.
- Model text is JSON-parsed without a schema validator.
- The form contains inconsistent field spelling between `noOfdays` and `nOofdays`, and a numeric conversion variable is not used when writing state.
- Error and loading paths are incomplete for several asynchronous operations.
- The initial Gemini chat history contains a large static Las Vegas sample.

## 10. Recommended future boundary

A production architecture should place privileged Gemini requests behind a server-side function. It should validate inputs, authenticate callers, enforce quotas, validate model output against a schema, and write data only after authorization. Firestore Security Rules must independently enforce ownership. The client should receive only the minimum data required to render the result.

Do not add this server boundary without updating deployment, API, security, privacy, and testing guides.
