# ⚙️ Environment Configuration

## 📌 Purpose and security boundary
Vite reads environment variables during development and build. Variables prefixed with `VITE_` are replaced into the client bundle and visible to anyone who can access the built assets. Never use a Vite-prefixed variable for a private credential.

## 📌 Variables referenced by the repository

| Variable | Source | Use | Security treatment |
|---|---|---|---|
| `VITE_GOOGLE_AUTH_CLIENT_ID` | `src/main.jsx` | Google OAuth browser client ID | Public client configuration, restrict authorized origins |
| `VITE_GOOGLE_GEMINI_AI_API_KEY` | `src/service/AIModal.jsx` | Gemini SDK in browser | Exposed browser key, move privileged requests to server |
| `VITE_GOOGLE_PLACES_API_KEY` | `.env.example` and inactive helper comments | Active use not verified | Do not assume autocomplete is connected; restrict if used |

## 🔹 Local configuration
Copy the example into a local file.

PowerShell:
```powershell
Copy-Item .env.example .env
```

macOS/Linux:
```bash
cp .env.example .env
```

Set placeholder values only in your local `.env`. Do not commit populated environment files. Restart Vite after changing variables.

## 🔹 Vite exposure model
Every `VITE_*` value referenced by frontend code can be inspected in JavaScript assets. Do not use these variables for service-account private keys, OAuth client secrets, server tokens, database passwords, signing keys, or unrestricted API credentials. Privileged operations belong on a trusted server-side function using server-only environment variables.

## 🔹 Provider restrictions
For browser API keys, restrict allowed origins/referrers and allowed APIs, use separate development and production keys, set quotas and budget alerts, monitor unexpected usage, and rotate a key if it has escaped its intended scope.

OAuth client IDs are meant to be public to browser applications, but authorized origins, consent settings, scopes, and callback behavior still need configuration.

## 📌 Firebase configuration
Firebase web configuration in `src/service/FirebaseConfig.jsx` is hard-coded. Firebase web API keys and app identifiers are generally public configuration, not authorization controls. Safety depends on Firestore Security Rules, Firebase Authentication, API restrictions, App Check where appropriate, and project settings.

## 📌 Troubleshooting
If OAuth or Gemini fails, verify exact variable spelling, confirm `.env` is at the repository root, restart Vite, and check the browser's Network and Console panels. Confirm the deployed host has the matching build-time values and rebuild after changes.

## 📌 Deployment
Configure public Vite variables in the host's build environment. Changing a variable after static assets were built does not retroactively change the bundle. Keep development and production provider projects separate where possible.

## 📌 Example placeholders
```dotenv
VITE_GOOGLE_AUTH_CLIENT_ID=replace-with-browser-oauth-client-id
VITE_GOOGLE_GEMINI_AI_API_KEY=replace-with-restricted-key
VITE_GOOGLE_PLACES_API_KEY=replace-with-restricted-key
```

This is a format example only. The current browser-based Gemini key is exposed; move privileged generation server-side before production.

## 📌 Checklist
- [ ] `.env` is ignored by Git.
- [ ] Only placeholders are committed in `.env.example`.
- [ ] No private credential uses a `VITE_` prefix.
- [ ] Browser keys are restricted by origin and API.
- [ ] Development and production configurations are separated.
- [ ] Quotas and budget alerts are reviewed.
- [ ] Production assets are rebuilt after configuration changes.
