# Project Roadmap

This roadmap describes engineering improvements for the current AI Trip Planner codebase. Items are proposals until implemented, reviewed, and tested.

## Current baseline

- React 18 and Vite 5 frontend.
- Browser routes for landing, trip creation, saved trips, and trip details.
- Google OAuth browser integration with profile data stored in local storage.
- Gemini SDK call from the browser.
- Firestore persistence in the `AITrips` collection.
- Places autocomplete dependency.
- Vercel rewrite for client-side routes.
- npm package manifest and lockfile.

## Priority 0: Security and ownership

- [ ] Move Gemini generation behind a trusted server endpoint.
- [ ] Rotate and restrict exposed provider keys.
- [ ] Add and review Firestore Security Rules.
- [ ] Enforce per-user document ownership using verified identity.
- [ ] Replace local-storage email as an authorization decision.
- [ ] Review Firebase Authentication integration and token lifecycle.
- [ ] Remove sensitive debug logging.

## Priority 1: Data reliability

- [ ] Define a versioned trip schema.
- [ ] Validate Gemini responses before Firestore writes.
- [ ] Normalize property naming and field casing.
- [ ] Add input constraints for destination and number of days.
- [ ] Use robust document identifiers.
- [ ] Handle missing, malformed, and partially generated itineraries.
- [ ] Add user-visible timeout, quota, retry, and provider failure behavior.

## Priority 2: Quality and accessibility

- [ ] Add component tests for critical forms and route views.
- [ ] Add end-to-end route tests.
- [ ] Add Firestore rules tests using Firebase Emulator Suite.
- [ ] Test OAuth cancellation and provider failures.
- [ ] Review keyboard operation and dialog focus handling.
- [ ] Improve empty, loading, and error states.
- [ ] Test responsive layouts and screen-reader semantics.

## Priority 3: Operations and performance

- [ ] Add dependency update automation and review advisories.
- [ ] Review bundle size and consider route-level code splitting.
- [ ] Optimize public images and remote image fallbacks.
- [ ] Add deployment verification steps.
- [ ] Define user data retention and deletion behavior.
- [ ] Document provider quotas and operational ownership.

## Out of scope unless separately designed

Real hotel reservations, availability confirmation, ticketing, payments, and travel-provider transactions require a distinct product, provider agreements, authorization model, and security review. Do not imply they are delivered by the current application.

## Prioritization principles

Security and data ownership take precedence over visual expansion. Do not mark roadmap items completed until code is merged and validation evidence exists. Keep documentation synchronized with the implementation.
