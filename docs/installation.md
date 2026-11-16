# Installation and Local Development

## Project overview
AI Trip Planner is a React application built with Vite. It provides a landing page, trip preference form, AI itinerary generation, Google sign-in, Firestore trip storage, saved trips, and trip details.

## Prerequisites
- Node.js compatible with Vite 5, preferably a current supported LTS release.
- npm, Git, and a modern browser.
- Access to the Google Cloud and Firebase projects for provider-backed features.

The repository uses npm and includes `package-lock.json`. Use npm commands. Do not introduce Bun or Yarn lockfiles.

Check versions:

```bash
node --version
npm --version
git --version
```

## Clone and install
```bash
git clone https://github.com/Brajesh31/AiTripPlanner.git
cd AiTripPlanner
npm ci
```

Use `npm ci` to install the dependency tree specified by the lockfile. Do not commit `node_modules/`; it is generated content and should be excluded by repository ignore rules.

## Configure environment
Copy the template:

PowerShell:
```powershell
Copy-Item .env.example .env
```

macOS / Linux:
```bash
cp .env.example .env
```

| Variable | Purpose | Exposure |
|---|---|---|
| `VITE_GOOGLE_AUTH_CLIENT_ID` | Google OAuth browser client ID | Public identifier |
| `VITE_GOOGLE_GEMINI_AI_API_KEY` | Gemini browser SDK key | Public in built bundle |
| `VITE_GOOGLE_PLACES_API_KEY` | Places-related key | Public in built bundle |

Vite exposes `VITE_*` variables to browser code. Never put a privileged server-only secret in one. The current Gemini integration uses a browser-side key and should be moved behind a server endpoint for production. Restrict browser keys by origin and enabled API.

Firebase configuration is currently hard-coded in `src/service/FirebaseConfig.jsx`. A Firebase web API key is not a replacement for security rules. Secure Firestore through authenticated ownership rules. Never commit a populated `.env` file.

## Run the development server
```bash
npm run dev
```

Open the exact local URL printed by Vite, normally `http://localhost:5173`. Restart Vite after changing environment values.

## Routes to verify
| Path | Expected screen |
|---|---|
| `/` | Landing page |
| `/create-trip` | Trip preferences and generation form |
| `/my-trips` | Saved trips |
| `/view-trip/:tripId` | Saved itinerary |

## Build and preview
```bash
npm run lint
npm run build
npm run preview
```

Vite emits the production build to `dist/`. Preview is for local verification, not production hosting.

## Common setup errors
- **Missing environment variable:** confirm `.env` is at repository root, spelling matches, and Vite was restarted.
- **OAuth origin error:** add the exact development origin to the Google OAuth client configuration.
- **Gemini request failure:** verify model access, key restrictions, quota, and API availability.
- **Firestore permission denied:** inspect identity and Firestore Security Rules. Never make the database publicly writable to bypass the error.
- **Deep-link 404:** confirm the deployment host applies the SPA fallback rewrite.

See [Troubleshooting](troubleshooting.md), [Environment Configuration](environment-configuration.md), and [Deployment](deployment.md).
