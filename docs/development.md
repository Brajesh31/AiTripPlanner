# Development Guide

## Development model
The project uses React 18, JSX, Vite, Tailwind CSS, and npm. Vite compiles the source for development and production. The current repository has no separate backend service.

## Source map
| Path | Purpose |
|---|---|
| `src/main.jsx` | React entry, providers, routes |
| `src/App.jsx` | Landing view |
| `src/components/custom/` | Header and hero |
| `src/components/ui/` | Radix-backed UI primitives |
| `src/create-trip/` | Trip preference form and generation |
| `src/my-trips/` | Saved trip list and cards |
| `src/view-trip/[tripId]/` | Trip detail screen |
| `src/service/` | Gemini, Firebase, and provider modules |
| `src/constants/` | Prompt and option definitions |
| `src/lib/` | Shared helpers |
| `public/` | Public static images |
| `vite.config.js` | Vite plugin and `@` alias |
| `tailwind.config.js` | Tailwind theme and content scanning |

## Commands
```bash
npm ci
npm run dev
npm run lint
npm run build
npm run preview
```

Use npm only. The committed lockfile is the reproducible dependency source.

## Import alias
Imports beginning with `@/` resolve to `src/`. The alias is configured in `vite.config.js` and `jsconfig.json`. Keep both definitions aligned.

## React conventions
- Use function components and hooks consistently.
- Give mapped elements stable keys.
- Keep component responsibilities focused.
- Avoid disabling lint rules globally to hide warnings.
- Handle asynchronous calls with explicit loading, success, and failure states.
- Validate route parameters before querying Firestore.
- Do not trust local storage or client-supplied email for authorization.
- Never log tokens, user profiles, or full travel plans unnecessarily.

## UI component conventions
Shared UI components live in `src/components/ui/` and use Radix primitives with Tailwind styling. Reuse them rather than copying modal, popover, button, or toast behavior. Review keyboard access, focus trapping/restoration, Escape behavior, labels, and screen-reader output.

## Provider integration
Keep provider calls within service modules where practical. The current code directly calls Gemini and Firestore from the browser. A server-side proxy requires corresponding changes to environment, API, architecture, security, and deployment documentation. Never expose privileged secrets in `VITE_*` variables.

## AI data handling
Treat Gemini output as untrusted text. Parse safely, validate against an explicit schema, enforce required fields and sensible bounds, and handle partial or malformed results. Do not render generated HTML directly. Verify travel information independently.

## Firestore access
Firestore Security Rules must enforce authenticated ownership. A query filtered by local-storage email is not a security control. Test allowed access to a user's own record and denied cross-user access.

## Before committing
```bash
npm run lint
npm run build
git diff --check
git status --short
```

Also test changed routes, provider failure states, responsive layouts, and keyboard navigation. Update relevant documentation when routes, providers, environment values, Firestore fields, authentication, or deployment behavior change.
