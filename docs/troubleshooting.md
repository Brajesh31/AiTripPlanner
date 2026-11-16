# Troubleshooting Guide

Diagnose issues with a repeatable environment and sanitized logs. Never share provider keys, OAuth access tokens, Firebase tokens, real user emails, or private itineraries.

## Installation and Node errors

Check:
```bash
node --version
npm --version
```
Use a Node version compatible with Vite 5. If dependency installation is corrupted, remove local `node_modules/` and rerun `npm ci`. Do not casually regenerate the committed lockfile.

## Vite cannot resolve an import

Confirm the import path, filename capitalization, package installation, and the `@/` alias in `vite.config.js` and `jsconfig.json`. Production hosts and CI environments may use case-sensitive filesystems.

## Environment variable is undefined

Confirm `.env` exists in the repository root and the variable name exactly matches the source. Only variables prefixed with `VITE_` are exposed to client code. Restart Vite after changing values. For deployment, set build-time variables in the host and rebuild.

## Google OAuth fails

Check the OAuth client ID, authorized JavaScript origins, consent screen configuration, enabled scopes, popup settings, and browser console. A localhost origin and production domain must be configured separately where required. Test with a non-production account.

## User profile request fails

The current code calls Google's User Info endpoint with an OAuth access token. Check the request status and token validity. Do not paste the access token into an issue. A localStorage profile object is not proof of identity and does not secure Firestore.

## Gemini generation fails

Check the Gemini API key restrictions, enabled API, quota, billing, network response, and model availability in current provider documentation. Remember the key is currently exposed in browser assets. Do not use an unrestricted production credential.

## Gemini output cannot be parsed

The application expects JSON text and calls JSON.parse. Model output can be empty, fenced, truncated, or structurally incomplete. Inspect sanitized response shape only, not private trip content. Validate the root object, hotel array, itinerary object, day entries, and places arrays before saving. See [AI Output Contract](ai-output-contract.md).

## Firestore permission denied

Check Firebase project selection, database initialization, Authentication identity, Security Rules, and document ownership. The current client filters by email from localStorage, which is not a security boundary. Do not loosen rules to public read/write as a quick fix.

## My Trips is empty

Check whether the user is signed in, the profile email is present, the document was written, and the Firestore query matches the stored field. Inspect sanitized Firestore errors. Check that the record belongs to the authenticated user under rules.

## Trip detail page says no document

Confirm the URL includes the expected trip ID and the Firestore document exists. Check access rules and network errors. Handle missing documents as an expected not-found state, not a blank page.

## Browser refresh on nested route returns 404

The host must serve `index.html` for React Router paths. The repository includes a Vercel rewrite, but the deployed configuration must be checked. Verify `/create-trip`, `/my-trips`, and `/view-trip/<id>` directly.

## Lint or build failure

Run commands locally:
```bash
npm ci
npm run lint
npm run build
```
Read the first actionable diagnostic. Do not disable lint rules globally to make CI green. Check Node version, lockfile, import capitalization, and environment-independent build-time assumptions.

## UI layout issue

Inspect responsive width, long destination strings, nested itinerary content, image sizing, and flex/grid constraints. Test at common phone widths and 200% zoom. Review keyboard focus and dialog layering.

## Report a new issue

Include browser/OS, route, commit, reproduction steps, expected/actual behavior, and sanitized Console/Network output. Use synthetic data. Never include credentials or private itineraries.
