# 🤝 Contributing Guide

Thank you for considering a contribution to AI Trip Planner. This repository is a React/Vite learning project that generates travel itinerary suggestions using Google and Firebase services. Contributions should improve correctness, usability, accessibility, reliability, security, or maintainability.

## 📌 1. Understand the project first

Read the [documentation index](docs/README.md), especially:
- [Architecture](docs/architecture.md)
- [Development](docs/development.md)
- [Environment Configuration](docs/environment.md)
- [AI Output Contract](docs/ai-output-contract.md)
- [Authentication](docs/authentication.md)
- [Firestore Data Model](docs/firestore-data-model.md)
- [Security](docs/security.md)
- [Testing](docs/testing.md)

The current app uses React 18, Vite 5, React Router, Tailwind CSS, Google OAuth, Gemini SDK, Firebase Firestore, and Axios. It does not contain a first-party backend API or a dedicated automated browser test suite.

## 📌 2. Good contribution areas

- form validation and input normalization;
- safe model output parsing and schema validation;
- user-friendly loading and error states;
- Firestore data ownership and rules;
- coherent authentication and session handling;
- responsive and accessible UI improvements;
- provider quota, failure, and timeout handling;
- dependency maintenance and build reliability;
- documentation and workflow maintenance.

Avoid adding dependencies or infrastructure without describing the problem they solve and the maintenance cost they introduce.

## 📌 3. Local setup

```bash
git clone https://github.com/Brajesh31/AiTripPlanner.git
cd AiTripPlanner
npm ci
```

Create a local `.env` from `.env.example` and configure development provider values. See [Environment Configuration](docs/environment.md). Do not use production user data or unrestricted API keys for local testing.

Start the development server:

```bash
npm run dev
```

## 📌 4. Development standards

- Keep React components focused and readable.
- Keep provider and persistence logic in appropriate service boundaries.
- Avoid putting privileged keys in browser code.
- Validate user input before provider calls.
- Treat model output as untrusted data.
- Use stable React keys.
- Handle promise failures, loading states, and empty results.
- Avoid broad ESLint suppressions unless justified with a comment.
- Keep user profile and trip data out of console logs.
- Ensure Firestore ownership is enforced by rules, not client-side filters.

## 📌 5. Provider and data changes

Changes to Gemini prompts or response fields must update the [AI Output Contract](docs/ai-output-contract.md), response validation, detail renderers, and compatibility expectations for already saved trips.

Changes to sign-in must update the [Authentication](docs/authentication.md) and [Privacy](docs/privacy.md) guides. Changes to Firestore must include access-rule review, ownership tests, and data lifecycle documentation.

Never commit OAuth tokens, Gemini keys, Firebase service account credentials, production database exports, or real itineraries.

## 📌 6. UI and accessibility expectations

For UI changes, test narrow and wide viewports, keyboard focus, labels, dialog behavior, selected states, error announcements, and reduced-motion settings. Generated itinerary cards must remain understandable when content is long, incomplete, or malformed.

## 📌 7. Validation requirements

Run:

```bash
npm ci
npm run lint
npm run build
npm run preview
```

Then test affected routes and failure scenarios. The repository does not currently contain a dedicated automated test suite. Do not claim unit or browser tests passed unless such tests have actually been added and run.

## 📌 8. Pull request requirements

A useful pull request describes:
- problem and user impact;
- technical approach;
- affected routes and services;
- environment or provider changes;
- data model or schema impact;
- security and privacy implications;
- validation commands and results;
- screenshots for meaningful UI changes;
- remaining limitations.

Use the pull request template in `.github/PULL_REQUEST_TEMPLATE.md`.

## 📌 9. Commit hygiene

Keep commits focused. Do not commit `.env`, `node_modules/`, `dist/`, editor caches, operating-system files, logs, credentials, or private user data. Keep `package-lock.json` synchronized with `package.json`.

## 📌 10. Asset licensing

Use original or appropriately licensed assets. Verify source, license, attribution, and commercial-use terms. Do not assume that an image found online can be redistributed.

## 📌 11. Maintainer

**Brajesh Kumar**  
GitHub: [@Brajesh31](https://github.com/Brajesh31)
