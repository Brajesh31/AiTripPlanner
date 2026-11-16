# Security Policy

## 1. Project security scope

AI Trip Planner is a browser-first React application integrating Google OAuth, Gemini, and Firebase Firestore. It handles profile information and saved travel itineraries. This document records security expectations and risks observed in the source. It is not a penetration-test report or a security certification.

## 2. Reporting vulnerabilities

Report suspected vulnerabilities privately to **Brajesh Kumar** ([GitHub @Brajesh31](https://github.com/Brajesh31)). Do not publish credentials, access tokens, private itineraries, exploit details, or personal data in a public issue.

Include:
- affected file, route, workflow, or provider integration;
- issue description and realistic impact;
- sanitized reproduction steps;
- browser and environment details;
- whether the issue requires authentication;
- evidence with secrets and personal data removed;
- suggested mitigation if known.

Reports are reviewed on a best-effort basis. No formal response-time commitment is promised.

## 3. Known risks requiring attention

### Gemini key exposure
`src/service/AIModal.jsx` reads `VITE_GOOGLE_GEMINI_AI_API_KEY` and initializes the Gemini SDK in browser code. Vite exposes `VITE_*` values in built client assets. This key is not private. Restrict it immediately and move privileged AI generation behind a trusted server-side function. Rotate it if it has been exposed beyond its intended scope.

### Firebase configuration and rules
`src/service/FirebaseConfig.jsx` contains Firebase web configuration values. These values are not an authorization boundary. Firestore Security Rules must enforce user ownership and field validation. The reviewed tree does not contain a Firestore rules file; treat secure rules as a production blocker.

### LocalStorage identity
The app stores Google profile data in localStorage and uses its email as a Firestore query filter. Browser storage can be forged. It must not authorize access. Align trusted authentication with Firestore rules, preferably using Firebase Authentication UID or a trusted backend.

### Model output
Gemini output is parsed with JSON.parse and written to Firestore without a full schema validator. Validate structure, bounds, allowed URL protocols, and document size before persistence and rendering.

## 4. Contributor security requirements

- Never commit keys, tokens, passwords, private keys, service account files, or production data.
- Do not put privileged credentials in Vite variables or browser bundles.
- Do not trust localStorage values for authorization.
- Do not render AI output as trusted HTML.
- Validate untrusted inputs and generated data.
- Keep provider scopes and API restrictions minimal.
- Use least-privilege GitHub Actions permissions.
- Avoid exposing secrets to untrusted pull-request workflows.
- Use separate development and production provider projects.
- Keep sensitive prompts, profiles, and itineraries out of logs.

## 5. Incident response

If a credential is exposed, revoke or rotate it immediately, inspect provider usage, and review relevant logs. Deleting the value in a later Git commit does not remove it from Git history.

If user trip data is exposed, investigate affected documents, access paths, provider logs, and applicable notification obligations. Preserve evidence securely and avoid publishing private itinerary contents.

## 6. Scope boundaries

The application provides itinerary suggestions and does not itself reserve accommodation or process payments. A vulnerability involving a third-party provider should still be assessed, but the absence of booking functionality is not a vulnerability.

## 7. Related documentation

- [Security Practices](docs/security.md)
- [Environment Configuration](docs/environment.md)
- [Authentication](docs/authentication.md)
- [Firestore Data Model](docs/firestore-data-model.md)
- [Privacy](docs/privacy.md)
