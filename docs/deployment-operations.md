# 🚀 Deployment Operations

## 📌 Purpose
This guide covers operations for the Vite single-page application. The repository contains `vercel.json` with a rewrite to `/index.html`, intended to support React Router browser-history paths. This does not prove that a Vercel project is connected or deployed successfully.

## 📌 Build settings
Typical static-host settings:
- Install command: `npm ci`
- Build command: `npm run build`
- Output directory: `dist`
- Project root: repository root unless deliberately configured otherwise

Use the provider's current settings and confirm the output directory.

## 📌 Environment values
Configure required `VITE_*` values in the build environment and rebuild after changes. These values are bundled into client assets. The OAuth ID is public client configuration. The Gemini key is currently used in browser code and exposed. The Places key's active use is not verified. Firebase config is currently hard-coded. No privileged secret becomes private by placing it in a Vite variable.

Use separate development and production provider projects where possible. Restrict keys and move privileged AI generation behind a server boundary before production.

## 📌 SPA route fallback
The application defines `/`, `/create-trip`, `/view-trip/:tripId`, and `/my-trips`. Refreshing a nested route must serve `index.html`, not return a static 404. The existing Vercel rewrite is intended to provide this behavior. Verify every route directly after deployment.

## 📌 Provider readiness
Before production:
- configure OAuth authorized origins and consent settings;
- restrict browser API keys;
- enable Gemini access and budget controls;
- verify Firestore project and Security Rules;
- align Google sign-in with trusted Firebase identity or a backend;
- confirm no development trip data exists in production.

## 📌 Pre-deployment checks
```bash
npm ci
npm run lint
npm run build
npm run preview
```
Test all four routes, direct navigation, refresh, sign-in, trip generation with a designated test account, Firestore save/list/detail behavior, and failure paths.

## 📌 Release procedure
1. Review the intended commit and full diff.
2. Confirm documentation and environment examples match source.
3. Confirm secrets and local files are not committed.
4. Run lint and build.
5. Review provider restrictions and Firestore rules.
6. Deploy the selected commit.
7. Verify OAuth origins and provider requests on the deployed domain.
8. Test a synthetic trip record.
9. Record deployed commit and environment.
10. Monitor quota, provider errors, and application failures.

## 📌 Rollback
Redeploy the last known-good commit or use provider rollback. Confirm OAuth origins and environment values match the selected version. Verify root and nested routes after rollback.

## 📌 Monitoring
Define who reviews build failures, provider quotas, unexpected AI usage, Firestore permission errors, and support reports. Avoid logging sensitive prompts, tokens, profiles, or private itineraries.

## 📌 Important boundary
Vercel rewrite supports frontend route fallback. It does not create a backend API, protect exposed API keys, secure Firestore data, or validate user identity.
