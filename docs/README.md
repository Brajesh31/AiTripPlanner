# 📚 AI Trip Planner Documentation

Welcome to the technical and operational handbook for **AI Trip Planner**. This project is a React/Vite application that creates AI-generated itinerary suggestions with Gemini and stores trip documents in Firebase Cloud Firestore.

> ⚠️ **Travel information notice:** This application is not a booking platform. AI-generated hotels, addresses, prices, ratings, images, opening hours, availability, transport times, ticket fees, safety, and accessibility details may be wrong or out of date. Verify important information with official providers before travelling.

## 🧭 Documentation map

| Guide | What you will learn |
|---|---|
| [🚀 Installation](installation.md) | Prerequisites, npm install, environment setup, and local startup |
| [🧑‍💻 Development](development.md) | Source structure, scripts, conventions, and local workflow |
| [🏗️ Architecture](architecture.md) | React entry, routes, components, providers, and service boundaries |
| [🔌 API and Services](api.md) | Gemini, Google OAuth, User Info, Places, and Firestore calls |
| [🗄️ Database](database.md) | Firestore collection, fields, read/write behavior, and lifecycle |
| [🌐 Deployment](deployment.md) | Vite build, output directory, SPA route fallback, and hosting |
| [🔐 Security](security.md) | Browser key exposure, identity, Firestore rules, and incident response |
| [🧯 Troubleshooting](troubleshooting.md) | Install, build, environment, OAuth, Gemini, Firestore, routes |
| [⚙️ Environment Configuration](environment.md) | Exact Vite variables and provider restrictions |
| [🤖 AI Output Contract](ai-output-contract.md) | Prompt input, JSON structure, parsing, and validation |
| [🔑 Authentication](authentication.md) | Google OAuth, local profile cache, and trusted identity |
| [🗃️ Firestore Data Model](firestore-data-model.md) | AITrips record shape and ownership requirements |
| [🛡️ Privacy](privacy.md) | Profile and itinerary handling, retention, and deletion |
| [♿ Accessibility](accessibility.md) | Forms, dialogs, keyboard, focus, and generated content |
| [⚡ Performance](performance.md) | Bundle cost, provider latency, images, and dependencies |
| [🚀 Deployment Operations](deployment-operations.md) | Host settings, provider setup, routes, release checks, rollback |
| [🧪 Testing](testing.md) | Lint/build baseline and manual integration test plan |
| [🧹 Maintenance](maintenance.md) | Dependency, provider, workflow, and repository upkeep |
| [❓ FAQ](faq.md) | Common project questions |

## 🧾 Current implementation snapshot

| Area | Current state |
|---|---|
| UI | React 18 with JSX |
| Build | Vite 5 |
| Routing | React Router DOM 6 |
| Styling | Tailwind CSS 3 and project CSS |
| Authentication | Browser-based Google OAuth integration |
| Profile cache | localStorage key `user` |
| AI generation | Gemini SDK called from browser code |
| Persistence | Firebase Cloud Firestore |
| Places | Package imported, current destination field is a standard input |
| First-party backend | Not present |
| Automated tests | No dedicated unit, integration, or browser suite identified |
| CI | ESLint and production build checks |
| Hosting config | Vercel rewrite to `index.html` |

## 🧭 Main application routes

| Route | Component | Purpose |
|---|---|---|
| `/` | `App` / `Hero` | Landing page |
| `/create-trip` | `CreateTrip` | Collect travel preferences and generate suggestions |
| `/view-trip/:tripId` | `ViewTrip` | Read and render one saved trip |
| `/my-trips` | `MyTrips` | List trips associated with the cached profile |

## 🔄 High-level user flow

1. User opens the landing page and chooses Create Trip.
2. The form collects destination, duration, budget, and traveler category.
3. If no cached profile exists, the user can sign in with Google OAuth.
4. The client requests profile data from Google's User Info endpoint.
5. User inputs are substituted into the AI prompt.
6. Browser code calls Gemini and receives text expected to contain JSON.
7. The app parses the response and writes user selection, trip data, email, and ID to Firestore.
8. The app navigates to the trip detail route.
9. The detail page reads and renders the saved record.
10. My Trips queries saved records using the cached profile email.

This is the current client-side flow, not a production-grade identity or authorization architecture.

## 🚨 Important engineering risks

- **Gemini key exposure:** `VITE_GOOGLE_GEMINI_AI_API_KEY` is used in browser code. Vite exposes `VITE_*` values in the built assets. Move privileged AI requests to a server-side function.
- **Firebase rules:** Firebase web configuration is not access control. The reviewed repository tree does not contain a Firestore rules file.
- **Client-controlled identity:** localStorage profile values can be forged. They must not authorize Firestore reads or writes.
- **Identity mismatch:** Google OAuth profile retrieval is not automatically a Firebase Authentication session.
- **Unvalidated model output:** Gemini text is parsed with JSON.parse but is not fully schema-validated before persistence.
- **Form state inconsistency:** the day-count property has inconsistent spelling in the current source.
- **Repository hygiene:** the repository tracks a large `node_modules/` tree. A deliberate bulk removal is still required while preserving `package-lock.json`.

## 🧰 How to use this handbook

- Starting locally? Read [Installation](installation.md) and [Environment](environment.md).
- Changing the routes or providers? Read [Architecture](architecture.md) and [API](api.md).
- Updating prompts or itinerary fields? Read [AI Output Contract](ai-output-contract.md), [Database](database.md), and [Firestore Data Model](firestore-data-model.md).
- Changing login or saved-trip access? Read [Authentication](authentication.md), [Privacy](privacy.md), and [Security](security.md).
- Preparing a release? Read [Testing](testing.md), [Deployment](deployment.md), and [Deployment Operations](deployment-operations.md).

## ✅ Documentation maintenance rules

1. Keep paths, scripts, variable names, routes, collection names, and field casing aligned with source code.
2. Clearly mark planned behavior as planned.
3. Never publish real credentials, OAuth tokens, user email addresses, or private itineraries in examples.
4. Treat every `VITE_*` variable as public browser configuration.
5. Record unresolved risks instead of implying that a policy file fixes them.
6. Update affected guides and this index whenever routes, data, auth, provider integration, or deployment changes.
