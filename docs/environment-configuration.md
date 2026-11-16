# Environment Configuration

## Configuration model

Vite loads environment values from files in the project root and replaces references at build time. Values prefixed with `VITE_` are available to browser application code and become part of the client bundle. Treat them as public values.

## Current template

The repository's `.env.example` lists:

| Name | Used for | Security classification |
|---|---|---|
| `VITE_GOOGLE_AUTH_CLIENT_ID` | Google OAuth browser client | Public client identifier |
| `VITE_GOOGLE_GEMINI_AI_API_KEY` | Gemini browser SDK initialization | Exposed browser key |
| `VITE_GOOGLE_PLACES_API_KEY` | Places configuration placeholder / browser integration | Exposed browser key |

The exact use of the Places key should be verified before assuming that every listed variable is consumed by active code. `src/service/GlobalApi.jsx` contains a commented-out HTTP helper.

## Local file setup

Copy `.env.example` to `.env` and enter development values. Do not commit the resulting `.env` file.

After changing environment values, restart the Vite development server. Vite reads environment values when starting/building the application.

## Do not put secrets in VITE variables

A variable such as `VITE_GEMINI_API_KEY` is not private. It can be recovered from the browser build and network calls. The current Gemini integration uses a browser key; this is a known production security risk.

For privileged services:
1. create a server-side endpoint or serverless function;
2. store the provider credential in the host's secret manager;
3. expose only the server endpoint to the browser;
4. validate caller identity and input;
5. apply rate and abuse limits;
6. avoid returning the secret to the client.

## Firebase configuration

The current Firebase configuration is hard-coded in `src/service/FirebaseConfig.jsx`. Moving project identifiers and web config to environment-specific values can make staging/production selection easier, but Firebase web configuration is not a secret boundary. Firestore Security Rules must protect the data.

## Google OAuth configuration

Use the OAuth client ID created for browser use. Configure authorized JavaScript origins for local development and production domains. Do not reuse a confidential server client secret in browser code.

## API key restrictions

For browser API keys:
- restrict by HTTP referrer;
- allow only required APIs;
- separate development and production keys;
- monitor quota usage;
- rotate keys if their restrictions are bypassed or the key is exposed outside its intended environment.

## Hosting configuration

Set production client variables in the Vercel project for the correct deployment environment. Server-only secrets, if introduced later, must not use a `VITE_` prefix. Rebuild after changing environment values because Vite injects them at build time.

## Validation checklist

- [ ] `.env` is ignored by Git.
- [ ] `.env.example` contains names and placeholders only.
- [ ] No server credential uses the `VITE_` prefix.
- [ ] OAuth origins match the current domain.
- [ ] Browser API keys are restricted.
- [ ] Firebase rules protect user trip documents.
- [ ] Production and development projects are not accidentally mixed.
- [ ] No credentials appear in build artifacts or logs.

See [Installation](installation.md), [Deployment](deployment.md), and [Security](security.md).
