# 📝 Changelog

This changelog records changes to the AI Trip Planner repository. It does not represent official Google, Firebase, or travel-provider product releases.

## 📌 Unreleased

### Added
- Expanded installation, architecture, API, database, and application-flow documentation.
- Added dedicated guides for environment variables, AI output contract, authentication, Firestore data model, privacy, accessibility, performance, deployment operations, testing, and maintenance.
- Expanded README with actual routes, stack, source layout, setup steps, and production limitations.
- Expanded contributor, security, support, code-of-conduct, and roadmap guidance.

### Changed
- Clarified the difference between Google OAuth profile data in localStorage and trusted Firebase Authentication identity.
- Documented that the Gemini key is exposed when used through a Vite client variable.
- Documented the need for Firestore Security Rules and verified ownership enforcement.
- Documented the fact that Google Places autocomplete is imported but the current location field is a standard input.
- Documented the current raw JSON parsing and lack of full schema validation for Gemini output.
- Clarified that AI suggestions are not verified prices, hotel availability, operating hours, or travel advice.

### Known risks and limitations
- The Gemini key is used in browser code and should be moved behind a server-side boundary.
- Firebase web configuration is hard-coded; Firestore rules and restrictions remain essential.
- localStorage profile values are client-controlled and must not authorize database access.
- No Firestore rules file was found in the reviewed repository tree.
- Gemini output is parsed without a complete schema validation layer.
- There is no dedicated unit, integration, or browser automation suite in the reviewed source tree.
- The repository tracks a very large `node_modules/` directory. A deliberate bulk cleanup remains necessary.
- The app does not book hotels or guarantee generated travel information.

## 📌 Earlier repository baseline

- React 18 and Vite single-page application.
- Google OAuth browser sign-in.
- Gemini-based itinerary-generation prototype.
- Firestore trip create, list, and detail operations.
- Vercel rewrite for browser-history routes.
