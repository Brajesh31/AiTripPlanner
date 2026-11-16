# Changelog

All notable repository changes should be recorded here. Entries describe this repository and its implementation, not external provider releases.

## Unreleased

### Documentation
- Expanded the README with current functionality, technology stack, routes, architecture, setup, environment, Firestore, AI output, deployment, validation, and known limitations.
- Added a documentation index and detailed installation, architecture, development, API, database, authentication, AI generation, data model, and environment configuration guides.
- Added dedicated testing, privacy, accessibility, performance, maintenance, release, troubleshooting, and FAQ guides.
- Expanded contribution, security, support, and community conduct policies.

### Repository hygiene
- Identified a large committed `node_modules/` tree containing generated dependency and Vite cache files.
- Planned removal of generated dependencies from version control while preserving the npm lockfile.
- Added a requirement to ignore local environment files, dependencies, build output, and editor/OS artifacts.

### Known implementation limitations
- Gemini is currently invoked from browser code using a `VITE_*` key.
- Firebase web configuration is hard-coded in client source.
- Local storage is used for a Google profile response.
- My Trips filters by client-provided email; this is not authorization.
- Firestore rules and rules tests are not included in the repository tree.
- AI output is parsed without complete runtime schema validation.
- No dedicated end-to-end or Firestore security test suite is present.
- The application does not implement booking, confirmed availability, or payment flows.

## Previous state
The repository already contained the React/Vite application, npm lockfile, Firebase and Google integration code, Vercel SPA rewrite, and initial project governance files.
