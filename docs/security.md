# Security Practices

## Scope and current risk
AI Trip Planner is a browser-first application integrating Google OAuth, Gemini, Google Places, and Firebase Firestore. It handles user profile information and saved travel itineraries. This document records risks found during source inspection. It is not a penetration-test report or security certification.

## Credential exposure
The Gemini key is read from `VITE_GOOGLE_GEMINI_AI_API_KEY` and used by browser code. Vite embeds `VITE_*` values in the client bundle. Anyone loading the built app can inspect them.

Firebase web configuration is hard-coded in `src/service/FirebaseConfig.jsx`. A Firebase web API key identifies a project and is not a substitute for authorization. Protect data with Firestore Security Rules and provider restrictions.

Actions:
- move Gemini calls to a server-side endpoint;
- store privileged Gemini credentials in the hosting secret manager;
- rotate credentials exposed beyond intended restrictions;
- restrict browser keys by referrer and enabled APIs;
- keep secrets out of Git, logs, screenshots, and client bundles.

## Authentication and local storage
The app stores a Google profile response in local storage and reads its email to query trips. Local storage can be edited and may persist after browser sessions. Do not use that email string as proof of identity. Use verified authentication and enforce authorization in Firestore rules or a trusted backend.

## Firestore authorization
No Firestore rules file was present in the inspected repository tree. Client-side query filters do not prevent a user from requesting another document directly. Require authenticated access, verify ownership for reads and writes, and deny unauthenticated access by default. Test cross-user access attempts.

## AI prompt and output handling
Treat model output as untrusted. Validate and bound user input, validate generated JSON against a schema, reject malformed or oversized fields, safely render text, never insert model-generated HTML directly, and do not treat prices, ratings, availability, or coordinates as verified facts.

## Privacy-aware logging
Remove debug logs that can expose OAuth profile responses, access tokens, prompts, or full itineraries. Use synthetic data in development and bug reports.

## Network and hosting
Use HTTPS. Restrict production OAuth origins and browser API keys. Configure suitable headers and Content Security Policy after verifying the required resources. CORS is not an authorization system.

## GitHub Actions
Use minimum workflow permissions. Avoid exposing secrets to untrusted pull-request code. Review third-party actions before upgrading.

## Vulnerability reporting
Report suspected issues privately using the root [SECURITY.md](../SECURITY.md). Do not include live keys, account tokens, private itineraries, or personal information in public issues.

## Production-readiness checklist
- [ ] Gemini requests are made through trusted server infrastructure.
- [ ] No privileged secrets are present in browser assets.
- [ ] Exposed credentials are rotated and restricted.
- [ ] Identity is verified by a trusted authentication system.
- [ ] Firestore rules enforce per-user ownership.
- [ ] Cross-user and unauthenticated access tests pass.
- [ ] AI responses are schema-validated.
- [ ] User input is validated and bounded.
- [ ] Sensitive data is excluded from logs.
- [ ] Retention and deletion behavior is documented.
- [ ] Provider quotas and abuse limits are configured.
