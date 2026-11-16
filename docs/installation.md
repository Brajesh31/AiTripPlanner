# 🚀 Installation and Local Development

## 📌 1. 🔹 Overview
AI Trip Planner is a React 18 and Vite application. It uses npm for dependency installation, Vite for development and production builds, React Router for navigation, Firebase for persistence, Google OAuth for sign-in, and the Google Generative AI SDK for itinerary generation.

This guide explains local setup. It does not provision Google Cloud, Firebase, or Vercel resources for you.

## 📌 2. Prerequisites

Install:
- Git;
- a supported Node.js LTS release compatible with the Vite 5 toolchain;
- npm (bundled with Node.js);
- a modern browser.

Confirm versions:

```bash
node --version
npm --version
git --version
```

The repository includes `package-lock.json`. Use `npm ci` to install the lockfile-resolved dependency tree. Do not delete or regenerate the lockfile as a routine troubleshooting step.

## 📌 3. Clone the repository

```bash
git clone https://github.com/Brajesh31/AiTripPlanner.git
cd AiTripPlanner
```

Confirm the working tree and package scripts:

```bash
git status --short
npm run
```

## 📌 4. Install dependencies

```bash
npm ci
```

The repository currently tracks a large `node_modules/` directory in Git. This is not required for reproducible installation and should be removed from repository history through a deliberate cleanup. A clean local install should still use `npm ci` and the committed lockfile.

Do not commit local dependency changes or generated Vite caches.

## 📌 5. Configure environment variables

Copy the example file:

### PowerShell

```powershell
Copy-Item .env.example .env
```

### macOS / Linux

```bash
cp .env.example .env
```

Set the provider values described in [Environment Configuration](environment.md):
- `VITE_GOOGLE_AUTH_CLIENT_ID`
- `VITE_GOOGLE_GEMINI_AI_API_KEY`
- `VITE_GOOGLE_PLACES_API_KEY`

The current source directly reads the Gemini key through `import.meta.env`. Google OAuth reads the client ID in `src/main.jsx`. The Places key is present in the example and a commented-out service helper, but that helper is not currently active.

**Security:** Vite exposes `VITE_*` values in browser assets. Do not put private credentials in these variables. The current browser-side Gemini key is visible to users. Use provider restrictions for development, and move privileged AI calls to a server-side function before production.

## 📌 6. Start the development server

```bash
npm run dev
```

Vite prints the local URL in the terminal, commonly `http://localhost:5173`. Use the exact URL printed by your local process.

To expose the dev server to a local network for device testing:

```bash
npm run dev -- --host 0.0.0.0
```

Do not expose a development server publicly with unrestricted provider keys.

## 📌 7. Available scripts

| Command | Purpose |
|---|---|
| `npm run dev` | Start Vite development server |
| `npm run build` | Create optimized production output in `dist/` |
| `npm run preview` | Preview the built output locally |
| `npm run lint` | Run ESLint using the repository's current script |

Run the checks before pushing:

```bash
npm run lint
npm run build
```

The lint command currently uses ESLint 8 and scans JavaScript/JSX files. Existing source contains disabled rules and unused imports; do not assume a passing lint run proves functional correctness.

## 📌 8. Application routes

The current router defines:

| Route | Component | Purpose |
|---|---|---|
| `/` | `App` | Landing and hero page |
| `/create-trip` | `CreateTrip` | Trip preference and generation flow |
| `/view-trip/:tripId` | `ViewTrip` | Read and render one Firestore trip |
| `/my-trips` | `MyTrips` | List trips for the cached signed-in profile |

The header is rendered outside the route provider in `src/main.jsx`. The application also mounts Google OAuth and a toast notification container.

## 📌 9. Provider setup checklist

Before testing sign-in or trip generation:
- create or identify the Firebase project;
- enable Firestore and configure Security Rules;
- configure the Google OAuth consent screen and browser client ID;
- add the correct local origin to OAuth authorized JavaScript origins;
- enable the Gemini API for the project used by the AI key;
- restrict browser-exposed keys by allowed origin and API where supported;
- configure billing and quotas as required by the provider;
- confirm the project has no production data in a development environment.

Never paste keys into public issues, screenshots, or chat logs.

## 📌 10. First-run verification

1. Start Vite and open the landing page.
2. Navigate to the trip creation route.
3. Confirm the header and form render.
4. Test the location field and autocomplete behavior.
5. Complete duration, budget, and traveler selections.
6. Test Google sign-in using a designated test account.
7. Generate a sample itinerary only after verifying the provider key and quota.
8. Confirm that a Firestore document is created.
9. Open the returned trip detail route.
10. Open My Trips and confirm the expected record appears for the signed-in account.

The application currently has limited error handling in several async paths. Use a non-production Firebase project and synthetic trip data while testing.

## 📌 11. Troubleshooting setup

See [Troubleshooting](troubleshooting.md) for dependency, Vite, environment, OAuth, Gemini, Firestore, and route issues. See [Environment Configuration](environment.md) for exact variable behavior.
