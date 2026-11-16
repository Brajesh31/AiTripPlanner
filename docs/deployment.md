# 🌐 Deployment Guide

## 📌 1. Build and hosting model

AI Trip Planner is a Vite single-page application. A production deployment must build the source and publish the generated `dist/` directory. Publishing the repository root as the site output without a build step will not produce the intended application.

The repository includes `vercel.json` with a rewrite to `/index.html`, allowing React Router to resolve browser-history routes on direct navigation and refresh.

## 📌 2. Prerequisites

- A reviewed commit on the deployment branch.
- Node.js and npm available in the build environment.
- Correct Vite build command and output directory.
- Google OAuth and API configuration.
- Firebase project configuration and Firestore Security Rules.
- A verified deployment domain with HTTPS.

## 📌 3. Build commands

```bash
npm ci
npm run lint
npm run build
```

Vite writes the production application to `dist/`. Configure the hosting provider to publish this directory.

## 📌 4. Vercel project configuration

Use the following expected settings:

| Setting | Value |
|---|---|
| Framework | Vite |
| Install command | `npm ci` |
| Build command | `npm run build` |
| Output directory | `dist` |
| Root directory | Repository root |

Confirm the production branch and ensure the deployment uses the intended project. The existing `vercel.json` rewrite sends client-side paths to `index.html`. Test that this does not interfere with static assets.

## 📌 5. Environment variables

The current client-side variables are:
- `VITE_GOOGLE_AUTH_CLIENT_ID`;
- `VITE_GOOGLE_GEMINI_AI_API_KEY`;
- `VITE_GOOGLE_PLACES_API_KEY`.

Set values in the correct Vercel environment. Vite variables are injected at build time, so changing them requires a new build/deployment.

**Security warning:** all `VITE_*` values are exposed to the browser. The current Gemini integration uses a browser key. This is not a recommended production boundary. Move model requests behind a server-side function and store its key as a server secret, without a `VITE_` prefix.

The current Firebase web configuration is hard-coded in `src/service/FirebaseConfig.jsx`. Firebase web config is not a secret authorization boundary. Secure the database with rules and verify project selection for production.

## 📌 6. Google OAuth configuration

Configure the Google OAuth client with the exact production JavaScript origin. Review authorized domains, consent configuration, and any redirect behavior required by the client library. Test sign-in success, cancellation, and provider errors on the deployed hostname.

Do not use development origins as production origins.

## 📌 7. Google Maps / Places configuration

Enable only required Maps Platform APIs, configure billing where required, and restrict browser keys to approved referrers and required APIs. Test autocomplete on the deployed hostname, not only localhost. Verify quota and error behavior.

## 📌 8. Firebase project and Firestore rules

Ensure the intended production Firebase project is selected. Deploy restrictive Firestore rules before enabling user traffic. Rules must enforce authenticated ownership for every trip document read and write.

No Firestore rules file was present in the inspected repository tree. This is an outstanding deployment requirement. Do not deploy unrestricted public read/write rules to make the UI work.

Test:
- authenticated owner can create and read their own trip;
- another user cannot read that trip;
- unauthenticated requests are denied;
- document ownership cannot be reassigned by editing client data.

## 📌 9. Post-deployment verification

- [ ] Landing route loads.
- [ ] All nested routes load directly and after refresh.
- [ ] OAuth works on the production domain.
- [ ] Trip form validates missing and invalid values.
- [ ] Gemini generation succeeds and handles provider failure.
- [ ] Firestore save and read work for an authorized user.
- [ ] Unauthorized cross-user reads/writes are denied.
- [ ] Malformed model output is rejected safely.
- [ ] No privileged key is present in built assets.
- [ ] Console and Network panels show no unexpected errors.
- [ ] Mobile and desktop layouts remain usable.
- [ ] HTTPS is active.

## 📌 10. Rollback

Record the deployed commit. If a release breaks routes or data access, redeploy the previous known-good build and verify it. If a key is exposed or user data was accessible, rollback alone is insufficient: rotate credentials, inspect provider usage, and correct access rules.

See [Environment Configuration](environment-configuration.md), [Security](security.md), and [Testing](testing.md).
