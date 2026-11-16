# AI Trip Planner Documentation

This directory contains the technical, operational, security, and maintenance documentation for **AI Trip Planner**. The guides describe the repository's current implementation and identify limitations that matter when developing or deploying it.

> **Important:** This is an AI-assisted itinerary planning application, not a travel booking platform. Generated hotel and destination suggestions can be incomplete, outdated, or inaccurate. The application does not make reservations or guarantee prices, hours, availability, transport times, safety, or accessibility.

## Documentation map

| Guide | Contents |
|---|---|
| [Installation](installation.md) | Requirements, npm installation, environment configuration, and local startup |
| [Development](development.md) | Source layout, code conventions, scripts, and workflow |
| [Architecture](architecture.md) | React entry point, routes, providers, UI layers, and service boundaries |
| [API and Services](api.md) | Gemini, Google OAuth, Google User Info, Places, and Firestore usage |
| [Database](database.md) | Firestore collection, document fields, query patterns, and data lifecycle |
| [Deployment](deployment.md) | Vite production build, Vercel SPA routing, environment setup, and release checks |
| [Security](security.md) | Browser key exposure, OAuth, Firebase rules, data ownership, and incident response |
| [Troubleshooting](troubleshooting.md) | Install, build, authentication, AI, Firestore, routing, and deployment issues |
| [Testing](testing.md) | Lint/build verification and a route/provider manual test plan |
| [Environment Configuration](environment.md) | Exact public Vite variables and provider restrictions |
| [AI Output Contract](ai-output-contract.md) | Prompt shape, expected JSON fields, parsing risks, and validation requirements |
| [Authentication and Identity](authentication.md) | Google OAuth browser flow and local profile cache |
| [Firestore Data Model](firestore-data-model.md) | AITrips records, ownership expectations, and security rules |
| [Privacy and Data Lifecycle](privacy.md) | Profile and itinerary data, retention, deletion, and provider sharing |
| [Deployment Operations](deployment-operations.md) | Environment parity, Vercel settings, route rewrites, rollback, and release verification |
| [Accessibility](accessibility.md) | Semantic markup, keyboard interactions, forms, dialogs, and screen readers |
| [Performance](performance.md) | Vite bundles, images, external requests, and runtime performance |
| [Contribution Guide](../CONTRIBUTING.md) | How to propose and validate changes |
| [Support](../SUPPORT.md) | What information to include when asking for help |
| [Roadmap](../ROADMAP.md) | Known engineering priorities and future work |

## Current application summary

AI Trip Planner is a React 18 single-page application using Vite. React Router handles the routes. The visible application flow includes:
1. a landing page and navigation;
2. a trip creation form for destination, duration, budget, and traveler group;
3. Google sign-in through the Google OAuth React package;
4. a Gemini model request using a prompt template;
5. saving the parsed itinerary and user selection into a Firestore `AITrips` collection;
6. listing the signed-in user's saved trip documents;
7. rendering a trip detail page from a Firestore document.

The app uses Tailwind CSS, Radix UI primitives, Lucide and React icon packages, Axios, Firebase, Google Places autocomplete, and the Google Generative AI SDK.

## Current technical boundaries

| Area | Current implementation |
|---|---|
| UI framework | React 18 with JSX |
| Dev/build tool | Vite 5 |
| Routing | React Router DOM 6 |
| Styling | Tailwind CSS 3 and project CSS |
| Authentication | Browser-based Google OAuth |
| Profile cache | Browser localStorage |
| AI generation | Gemini SDK called from browser code |
| Persistence | Cloud Firestore |
| Places | Google Places autocomplete package is imported in the trip form; a separate Places API helper is currently commented out |
| Backend API | No first-party backend service in this repository |
| Hosting configuration | Vercel rewrite to `index.html` |
| Tests | No dedicated unit or browser test suite currently present |

## Important security and reliability notes

The source contains Firebase web configuration values in `src/service/FirebaseConfig.jsx`. Firebase web configuration is normally public client configuration, but its safety depends on correct Firebase Security Rules, API restrictions, and project configuration. It must not be treated as authorization.

The Gemini API key is read from `VITE_GOOGLE_GEMINI_AI_API_KEY` and used in browser code. Any Vite environment variable beginning with `VITE_` is bundled into client-side output and can be inspected by visitors. A privileged generative-AI key should be moved behind a trusted server or serverless function before production use. Rotate any credential that has been exposed beyond its intended scope.

The application stores a Google profile object in localStorage and associates saved trips using the email field. Client-side checks are not access control. Firestore Security Rules must enforce authenticated ownership. Review [Security](security.md), [Privacy](privacy.md), and [Firestore Data Model](firestore-data-model.md) before deployment.

## How to read the docs

Use the installation guide to start locally. Read architecture and service docs before changing cross-cutting behavior. For changes involving user identity or stored trips, review authentication, Firestore, privacy, and security together. For prompt or response changes, update the AI output contract and corresponding UI renderers.

Do not document planned behavior as implemented. A feature is current only when the source code implements it and its behavior has been verified.

## Documentation maintenance standards

- Keep paths, scripts, variable names, route names, and collection names aligned with source.
- State whether a provider integration is active, experimental, or commented out.
- Never include real API keys, OAuth tokens, user emails, or itinerary contents in examples.
- Describe browser-visible Vite variables as public configuration, never as secrets.
- Record unresolved risks instead of implying that a policy file fixes them.
- Update related documentation in the same change when routes, data, environment variables, or deployment behavior change.
