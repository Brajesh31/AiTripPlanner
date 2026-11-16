# 🛣️ AI Trip Planner Roadmap

This roadmap tracks practical product and engineering improvements. Items are proposals until implemented, tested, and documented. Security and data ownership work takes priority over adding new visual features.

## 📌 Completed foundation

- [x] React and Vite application entry point.
- [x] React Router routes for home, create trip, trip detail, and saved trips.
- [x] Google OAuth browser sign-in flow.
- [x] Gemini-based itinerary generation prototype.
- [x] Firestore trip document create, list, and detail read paths.
- [x] Vercel SPA fallback configuration.
- [x] Project-specific technical documentation and environment examples.
- [x] Security, privacy, authentication, and AI output limitations documented.
- [x] Detailed testing and deployment guidance added.

## 📌 Priority 0: security and data ownership

- [ ] Move privileged Gemini requests behind a server-side endpoint.
- [ ] Rotate/restrict exposed browser keys and remove privileged provider access from client bundles.
- [ ] Align Google sign-in with Firebase Authentication or a trusted server identity.
- [ ] Add and emulator-test Firestore Security Rules based on authenticated UID.
- [ ] Define trip ownership, deletion, account closure, and retention behavior.
- [ ] Remove sensitive profile, token, prompt, and itinerary data from debug logs.

## 📌 Priority 1: correctness and reliability

- [ ] Normalize and validate destination, duration, budget, and traveler fields.
- [ ] Resolve inconsistent day-count spelling in form state.
- [ ] Validate model output with a stable schema before Firestore writes.
- [ ] Handle timeouts, rate limits, quota errors, malformed JSON, and permission failures.
- [ ] Add clear loading, empty, success, and error states.
- [ ] Handle missing trip documents and incomplete historical records safely.
- [ ] Replace timestamp document IDs with Firestore-generated IDs or a suitable unique ID strategy.
- [ ] Add tests for trip form input and AI response parsing.

## 📌 Priority 2: user experience and accessibility

- [ ] Make destination autocomplete genuinely active or remove unused imports/configuration.
- [ ] Make budget and traveler selections fully keyboard-operable with selected state.
- [ ] Improve sign-in dialog accessible title and focus behavior.
- [ ] Improve mobile layouts and long itinerary content.
- [ ] Add meaningful not-found and empty saved-trip states.
- [ ] Add trip deletion and user data controls after the ownership model is secure.
- [ ] Label generated suggestions and require verification of travel details.

## 📌 Priority 3: maintainability and operations

- [ ] Remove tracked `node_modules/` and generated Vite cache from Git while preserving package-lock.json.
- [ ] Add CI for npm ci, lint, and build.
- [ ] Add browser smoke tests for route navigation and refresh.
- [ ] Add Firestore Emulator security-rule tests.
- [ ] Review dependency duplication and unused packages.
- [ ] Review provider model versions, quotas, and SDK lifecycle.
- [ ] Add operational monitoring without logging sensitive itineraries.
- [ ] Document backup and restore procedure.

## 📌 Non-goals without a separate product decision

- Direct hotel reservations or payment processing.
- Claims that generated prices or availability are guaranteed.
- Collecting passport or payment-card details.
- Treating browser localStorage as trusted identity.
- Adding a backend or complex infrastructure solely for appearance.

## 📌 Prioritization policy

Fix data exposure and authorization before expanding user features. Fix invalid model output handling before claiming reliable trip generation. Do not mark roadmap items complete until source changes are committed and relevant tests have been run.
