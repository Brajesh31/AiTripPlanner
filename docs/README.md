# AI Trip Planner Documentation

This directory is the technical and operational reference for **AI Trip Planner**, a React/Vite application that generates itinerary suggestions using Gemini and stores trip documents in Firebase Cloud Firestore.

> **Important:** The application is not a booking platform. AI-generated hotel and destination details may be inaccurate or outdated. Prices, opening hours, availability, travel times, ratings, images, ticket costs, safety, and accessibility must be verified with official providers.

## Guide index

| Guide | Purpose |
|---|---|
| [Installation](installation.md) | Prerequisites, npm installation, local configuration, and startup |
| [Development](development.md) | Source tree, scripts, conventions, and local workflow |
| [Architecture](architecture.md) | React entry point, providers, routes, components, and service boundaries |
| [API and Services](api.md) | Gemini, Google OAuth, User Info, Places, and Firestore integrations |
| [Database](database.md) | Current Firestore collection, fields, read/write behavior, and lifecycle |
| [Deployment](deployment.md) | Vite build, output directory, SPA route fallback, and hosting |
| [Security](security.md) | Exposed browser key, identity, Firestore rules, and incident handling |
| [Troubleshooting](troubleshooting.md) | Install, build, environment, OAuth, Gemini, Firestore, routing, and deployment |
| [Environment](environment.md) | Exact Vite variables and provider restrictions |
| [AI Output Contract](ai-output-contract.md) | Prompt input, expected JSON structure, and validation requirements |
| [Authentication](authentication.md) | Google OAuth, local profile cache, and trusted identity boundaries |
| [Firestore Data Model](firestore-data-model.md) | AITrips document shape and owner access expectations |
| [Privacy](privacy.md) | Profile and itinerary handling, retention, and deletion |
| [Accessibility](accessibility.md) | Forms, dialogs, keyboard, focus, and generated content |
| [Performance](performance.md) | Bundle cost, provider latency, images, and dependency hygiene |
| [Deployment Operations](deployment-operations.md) | Host settings, provider setup, route verification, rollback |
| [Testing](testing.md) | Lint/build baseline and manual integration test plan |
| [Maintenance](maintenance.md) | Dependency, provider, workflow, and repository maintenance |
| [FAQ](faq.md) | Frequently asked project questions |

## Current implementation snapshot

| Area | Status |
|---|---|
| UI | React 18 with JSX |
| Build | Vite 5 |
| Routing | React Router DOM 6 |
| Styling | Tailwind CSS 3 and project CSS |
| Authentication | Browser-based Google OAuth library |
| Profile storage | localStorage key `user` |
| AI | Gemini SDK called in browser |
| Persistence | Firebase Cloud Firestore |
| Places | Package imported, but current destination input is a text input |
| First-party API | Not present |
| Automated tests | No unit, integration, or browser test suite identified |
| CI | Lint and build workflow |
| Hosting | Vercel rewrite to `index.html` |

## Primary routes

- `/`: landing page.
- `/create-trip`: collect destination, number of days, budget, and traveler group.
- `/view-trip/:tripId`: fetch a Firestore trip and render it.
- `/my-trips`: query trip documents associated with the cached profile email.

## Data flow

The user enters preferences, signs in with Google when no cached profile is present, and the client retrieves a Google profile. The form builds a prompt and sends it to Gemini. Returned text is parsed with JSON.parse and written to Firestore with user selection, trip data, email, and document ID. The application then navigates to the trip detail route. My Trips queries Firestore using the cached profile email.

This is a description of current browser code, not a secure production identity architecture.

## Important known risks

- `VITE_GOOGLE_GEMINI_AI_API_KEY` is used in browser code. Vite embeds VITE variables into browser assets. This key is exposed. Move privileged AI generation behind a server-side function and rotate keys as necessary.
- Firebase web configuration is hard-coded. It is public client configuration, not access control.
- localStorage profile values can be forged. They must not authorize Firestore access.
- The Google OAuth client profile is not shown as a Firebase Authentication session.
- The reviewed repository tree does not contain Firestore Security Rules. Do not store real user data until rules enforce authenticated ownership.
- Gemini output is parsed without full schema validation.
- The form currently has inconsistent duration state spelling and input conversion handling.
- Error handling for asynchronous provider and database operations is incomplete.
- The project tracks a large `node_modules/` directory in Git. This should be removed in a deliberate bulk cleanup while retaining `package-lock.json`.

## Documentation maintenance

When routes, environment variables, provider SDKs, data fields, prompt schema, authentication, or deployment settings change, update the affected guides and this index. Mark proposed behavior as planned until it is implemented and verified. Never include real credentials, OAuth tokens, user email addresses, or private travel plans in public examples.
