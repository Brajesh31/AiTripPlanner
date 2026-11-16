# Troubleshooting Guide

## Installation and Node errors
Check `node --version` and `npm --version`. Use a Node version compatible with Vite 5. When troubleshooting a broken install, remove local `node_modules/` and run `npm ci` again. Do not casually regenerate the committed lockfile.

## Vite cannot resolve an import
Check spelling and filename capitalization, confirm the file exists under `src/`, and verify the `@` alias in `vite.config.js` and `jsconfig.json`. Restart Vite after config changes.

## Missing environment values
Confirm `.env` is in the repository root and names match `.env.example`. Restart the dev server after changes. Remember that `VITE_*` values are embedded in browser code.

## Google OAuth origin error
Check OAuth client ID, authorized JavaScript origins, consent configuration, and actual browser URL including scheme and port. Configure localhost and production domains separately.

## Google profile request fails
Inspect OAuth response, token scope, network request, and provider error. Do not log or share access tokens. Handle cancellation and denied permissions as recoverable states.

## Gemini request fails
Check key validity, API enablement, model access, quota, billing, and network restrictions. Confirm model identifier support for the configured SDK. Never expose private keys in public issues.

## Gemini returns invalid JSON
Model output can include markdown fences, extra prose, wrong field names, or missing arrays. Treat it as untrusted. Use schema validation, show a recoverable error, and do not write invalid data to Firestore. Apply a retry limit.

## Firestore permission denied
Check Firebase project configuration, identity, and Firestore Security Rules. Do not use public read/write rules as a workaround. Client-side email filtering is not authorization.

## My Trips appears empty
Confirm profile state, Firebase project, saved document fields, and rules. Inspect console and network errors.

## Trip details cannot find a record
Check the `tripId` path segment, Firestore document ID, project config, and document existence. Handle missing or malformed data without crashing.

## Places autocomplete does not work
Verify Maps Platform APIs, billing, browser key restrictions, allowed referrers, and network errors. The helper in `GlobalApi.jsx` is commented out and inactive.

## Deep route returns 404
Check `vercel.json` and host rewrite configuration. Client routes should resolve to `index.html`. Confirm deployment output directory is `dist/`.

## Lint fails
Run `npm run lint` and fix reported issues. Review stale imports, unused variables, missing React keys, and hook dependencies. Avoid broad rule suppression.

## Build fails
Run `npm run build` and inspect the first meaningful error. Check missing imports, JSX syntax, alias resolution, and package-lock consistency. Lint passing does not guarantee build success.

## Useful bug report information
Include route, browser/version, OS, Node/npm versions, commit, reproduction steps, expected/actual behavior, sanitized console/network output, and whether a clean install reproduces it. Never include keys, tokens, profile data, or private itineraries.
