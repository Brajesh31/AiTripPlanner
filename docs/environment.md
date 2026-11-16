# Environment Configuration

## 1. Purpose

Vite reads environment variables during development and build. This project uses variables prefixed with `VITE_`. Vite replaces these references in the client bundle, which means their values are visible to anyone who can access the application assets.

**Never use a `VITE_*` variable for a private credential.** Browser-side API keys must be treated as public and protected through provider restrictions, quotas, and appropriate architecture.

## 2. Variables currently referenced

| Variable | Source reference | Current use | Handling |
|---|---|---|---|
| `VITE_GOOGLE_AUTH_CLIENT_ID` | `src/main.jsx` | Google OAuth browser client ID | Public client configuration; restrict authorized origins and OAuth settings |
| `VITE_GOOGLE_GEMINI_AI_API_KEY` | `src/service/AIModal.jsx` | Gemini SDK instantiated in the browser | Exposed client key; move privileged AI calls to a server-side boundary |
| `VITE_GOOGLE_PLACES_API_KEY` | Present in `.env.example` and inactive helper comments | Current active use is not verified | Do not assume autocomplete is connected; restrict any browser key to required APIs and origins |

The actual example file contains placeholder values. Replace them locally only. Do not commit a populated `.env` file.

## 3. Local file setup

PowerShell:

```powershell
Copy-Item .env.example .env
```

macOS / Linux:

```bash
cp .env.example .env
```

After changing environment values, restart the Vite development server. Vite reads environment configuration when the process starts and during build.

## 4. Vite exposure model

Vite exposes variables with the configured public prefix to browser code. A variable such as `VITE_GOOGLE_GEMINI_AI_API_KEY` is not private merely because it is stored in a local `.env` file. Once built, the value can be inspected in JavaScript assets.

Do not use Vite variables for:
- service account private keys;
- OAuth client secrets;
- server-side access tokens;
- database passwords;
- unrestricted provider API keys;
- signing keys or encryption secrets.

For privileged operations, use a server-side function and a server-only environment variable without the public Vite prefix.

## 5. Provider restrictions

For Google browser keys:
- restrict allowed web origins or HTTP referrers;
- restrict the key to the minimum required APIs;
- use separate keys for development and production;
- set quotas and budget alerts;
- rotate keys if their intended scope is exceeded;
- review usage and unexpected traffic.

OAuth client IDs are designed to be used by browser applications, but authorized origins, consent settings, redirect behavior, and application domains must still be configured correctly.

## 6. Firebase configuration

The current Firebase web configuration is hard-coded in `src/service/FirebaseConfig.jsx`. Firebase web configuration values, including the web API key, are commonly public identifiers, not authorization controls. Their safety depends on Firebase Security Rules, Authentication, App Check where appropriate, API restrictions, and project configuration.

Do not replace this configuration with a client-side secret. If moving values into Vite environment variables for environment separation, remember that they remain public.

## 7. Missing or incorrect values

Symptoms include:
- Google sign-in does not initialize;
- Gemini reports authentication or API errors;
- provider requests fail due to restrictions;
- the browser bundle receives `undefined` for a configuration value.

Check exact variable spelling, ensure `.env` is in the project root, restart Vite, and verify the correct deployment environment contains the matching values.

## 8. Deployment environment

Configure public Vite variables in the hosting provider's build environment and trigger a fresh build after changes. Do not assume changing a runtime environment variable after the static assets have been built will change values already embedded in the bundle.

Keep development and production keys and provider projects separate where practical.

## 9. Example policy

```dotenv
VITE_GOOGLE_AUTH_CLIENT_ID=replace-with-your-browser-oauth-client-id
VITE_GOOGLE_GEMINI_AI_API_KEY=replace-with-a-restricted-browser-key
VITE_GOOGLE_PLACES_API_KEY=replace-with-a-restricted-browser-key
```

This is a placeholder format, not a recommendation to expose privileged Gemini credentials from the browser. The Gemini integration should be moved server-side for a production deployment.

## 10. Review checklist

- [ ] `.env` is ignored by Git.
- [ ] Only placeholders are committed in `.env.example`.
- [ ] No private service credential uses a `VITE_` prefix.
- [ ] Browser API keys are restricted by origin and API.
- [ ] Development and production environments are separated.
- [ ] Provider quotas and budget alerts are reviewed.
- [ ] A production build is regenerated after environment changes.
