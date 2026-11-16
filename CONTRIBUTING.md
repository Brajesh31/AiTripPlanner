# 🤝 Contributing Guide

Thank you for considering a contribution to AI Trip Planner. The repository is a React/Vite travel-planning application that connects Google OAuth, Gemini, Google Places autocomplete, and Firebase Firestore. Contributions should make the application more correct, secure, accessible, reliable, and maintainable.

## 1. Project scope

Current code includes:
- React route screens for landing, trip creation, saved trips, and trip detail;
- Google OAuth browser integration;
- Gemini-assisted generation of itinerary suggestions;
- Firestore persistence in the `AITrips` collection;
- Google Places autocomplete integration;
- Tailwind styling and Radix UI components.

The application does not provide hotel reservation, confirmed availability, ticket sales, or payment processing. AI-generated prices, ratings, images, coordinates, and travel guidance are not verified facts.

## 2. Before starting

1. Read the [documentation index](docs/README.md).
2. Search existing issues and discussions.
3. Identify the user problem and affected route.
4. Check whether the change affects provider configuration or persisted Firestore fields.
5. Confirm that any media or external service is permitted for the intended use.
6. Keep the change focused.

## 3. Local setup

```bash
git clone https://github.com/Brajesh31/AiTripPlanner.git
cd AiTripPlanner
npm ci
cp .env.example .env
npm run dev
```

On Windows PowerShell, copy the example using `Copy-Item .env.example .env`. Configure development provider values locally. Do not commit populated environment files.

## 4. Code standards

- Follow the existing React and JSX conventions.
- Keep route-level functionality in its feature folder.
- Keep shared UI primitives reusable.
- Keep provider integration in service modules.
- Preserve Vite's `@` import alias.
- Use stable React keys.
- Handle async work with explicit loading, success, and failure states.
- Validate route parameters and external data.
- Avoid disabling lint rules broadly.
- Do not log access tokens, user profiles, or full itineraries.
- Do not use local-storage email as authorization.
- Treat Gemini output as untrusted until schema-validated.

## 5. Dependencies

Use npm and update `package-lock.json` with package changes. Do not add Bun or Yarn lockfiles. Avoid adding a dependency for a behavior that can be handled clearly with existing utilities or browser APIs. Review dependency license, maintenance, security advisories, size, and compatibility before adoption.

Do not commit `node_modules/`, Vite cache, `dist/`, or other generated files.

## 6. Provider and data changes

Any change to OAuth, Gemini, Places, Firebase, or Firestore must document:
- required environment values and whether they are browser-visible;
- provider permissions and API restrictions;
- request/response shape;
- quota, timeout, retry, and error behavior;
- privacy and retention implications;
- authorization enforcement;
- test coverage.

A change to persisted Firestore fields requires compatibility consideration for existing records. Update [Data Model](docs/data-model.md) and [Database](docs/database.md).

## 7. Validation

Before submitting:

```bash
npm ci
npm run lint
npm run build
git diff --check
```

Also test changed routes in a browser. For provider flows, test success and failure states. For UI changes, test responsive layouts and keyboard access. For Firestore changes, verify both authorized and unauthorized access using a dedicated test project.

The repository currently has no dedicated end-to-end or Firestore rules test suite. Do not claim such tests passed unless they have actually been added and run.

## 8. Pull request description

Explain:
- problem and motivation;
- implementation approach;
- routes and files affected;
- UI or data behavior change;
- environment/provider changes;
- tests and environments used;
- known limitations;
- security/privacy considerations.

Include screenshots for visible UI changes when useful, but do not include personal profiles, access tokens, private itineraries, or other sensitive data.

## 9. Commit hygiene

Keep commits focused and descriptive. Do not commit `.env`, `node_modules/`, build output, IDE caches, private logs, credentials, or customer data. Inspect `git status` and the complete diff before committing.

## 10. Review principles

Changes are reviewed for correctness, security, accessibility, data ownership, maintainability, licensing, and consistency with documented behavior. A change may require revision if it introduces unvalidated model output, weakens Firestore rules, exposes keys, or claims that an unimplemented feature exists.

## 11. Maintainer

**Brajesh Kumar**  
GitHub: [@Brajesh31](https://github.com/Brajesh31)
