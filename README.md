# AI Trip Planner

AI Trip Planner is a React and Vite travel-planning interface with Google authentication, Firebase persistence, Google Places autocomplete, and Gemini-assisted itinerary generation.

## Stack

- React 18 and Vite
- React Router, Tailwind CSS, Radix UI, Lucide
- Google OAuth
- Firebase
- Google Places and Gemini APIs

## Setup

```powershell
npm ci
Copy-Item .env.example .env
npm run dev
```

Required Vite variables are listed in `.env.example`. Because Vite exposes `VITE_*` values to the browser, use restricted public client keys and provider-side referrer restrictions. Do not place privileged secrets in frontend code.

## Scripts

```powershell
npm run dev
npm run build
npm run lint
npm run preview
```

## Security note

Review `src/service/FirebaseConfig.jsx` before deployment. Firebase configuration and any API key embedded in source should be rotated and moved to appropriate environment configuration where applicable. Never commit private credentials or user travel data.

See [docs/architecture.md](docs/architecture.md), [docs/development.md](docs/development.md), and [docs/security.md](docs/security.md).

Maintained by Brajesh Kumar (`brajesh31`).

Last updated: 2026-10-27 04:33 UTC
