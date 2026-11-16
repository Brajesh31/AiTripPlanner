# 🧑‍💻 Development Guide

## 📌 Development model

AI Trip Planner is a React 18 JSX application built with Vite 5 and npm. Vite serves the application during development and creates the production assets. There is no first-party backend service in the repository.

## 📌 Source map

| Path | Responsibility |
|---|---|
| `src/main.jsx` | React entry, providers, routes, and toaster |
| `src/App.jsx` | Landing page component |
| `src/components/custom/` | Header and hero components |
| `src/components/ui/` | Button, dialog, input, popover, toast primitives |
| `src/constants/Options.jsx` | Traveler options, budget options, and AI prompt |
| `src/create-trip/index.jsx` | Form state, OAuth, Gemini request, Firestore save |
| `src/my-trips/index.jsx` | Saved-trip query and listing |
| `src/my-trips/components/` | Saved-trip card |
| `src/view-trip/[tripId]/Index.jsx` | Detail document fetch and page composition |
| `src/view-trip/[tripId]/components/` | Hotel, itinerary, info, place, footer components |
| `src/service/AIModal.jsx` | Gemini client and chat session |
| `src/service/FirebaseConfig.jsx` | Firebase initialization and Firestore export |
| `src/service/GlobalApi.jsx` | Currently commented Places API helper |
| `src/index.css` | Tailwind directives and global tokens |
| `src/App.css` | Landing component styles |
| `src/lib/utils.js` | Utility class merger |

## 📌 Scripts

- `npm run dev`: development server.
- `npm run build`: production build into `dist/`.
- `npm run preview`: serve the production build locally.
- `npm run lint`: run ESLint with the current package script.

## 📌 Recommended workflow

1. Pull the latest main branch.
2. Read relevant architecture and service documentation.
3. Inspect existing state and provider code before editing.
4. Make one focused change.
5. Run lint and build.
6. Preview the production build.
7. Test the affected routes and provider failures.
8. Check keyboard, responsive layout, and screen-reader behavior for UI changes.
9. Update docs and changelog when behavior or configuration changes.
10. Review the diff and ensure no secrets or generated files are included.

## 📌 Component conventions

Keep components focused on one view or reusable concern. Put provider operations in service modules rather than duplicating SDK setup throughout components. Keep prompt constants and recognized option values centralized. Use stable keys for mapped JSX. Prefer state values with consistent naming and types.

Do not disable React hook or accessibility lint rules broadly to suppress warnings. Fix dependencies and state ownership where possible, documenting narrowly scoped exceptions if unavoidable.

## 📌 Async operations

Provider and Firestore requests can fail. Each operation should have explicit pending, success, empty, and failure states. Avoid unhandled promise rejections. Prevent duplicate generation requests and stale state updates after route changes. Do not log tokens, full user profiles, or complete private itineraries.

## 📌 Form validation

Validate destination length, supported duration range, integer day counts, budget choices, and traveler categories before calling Gemini. Use one canonical field name for day count throughout input state, prompt interpolation, Firestore persistence, and rendering. Do not trust values merely because the UI normally supplies them.

## 📌 Gemini output handling

Model response text is untrusted. Parse it in one service boundary, validate required fields and data size, reject malformed content, and only then persist. Do not render generated content as raw HTML or trust model-supplied links and facts.

## 📌 Firestore access

Client-side filtering by email is not authorization. Firestore Security Rules must use a trusted identity and prevent cross-user reads and writes. Review authentication and data model docs together before changing persistence.

## 📌 Local validation

```bash
npm ci
npm run lint
npm run build
npm run preview
```

## 📌 Documentation obligations

Changes to routes require updates to architecture and deployment. Changes to public variables require updates to environment docs and .env.example. Changes to prompt fields require updates to AI output, database, and renderers. Changes to auth or ownership require updates to authentication, Firestore, privacy, and security docs.
