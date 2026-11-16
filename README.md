# ✈️ AI Trip Planner

**AI Trip Planner** is a React and Vite application for creating personalized travel itineraries with generative AI. Visitors can choose a destination, trip duration, budget, and traveler group, sign in with Google, generate an itinerary, save it in Firebase Firestore, revisit saved trips, and view hotel and daily place recommendations.

<p align="center">
  <img src="public/nirvananomads.png" alt="Nirvana Nomads application logo" width="190">
</p>

<p align="center">
  A travel-planning assistant that turns preferences into a structured trip draft.
</p>

> [!IMPORTANT]
> AI-generated travel plans are suggestions, not verified reservations or live inventory. Hotel details, prices, ratings, coordinates, image URLs, travel times, and opening information may be incomplete or inaccurate. Confirm important details with official providers before relying on them. This application is an independent project and is not affiliated with Google, Firebase, Gemini, or any travel provider.

## 📚 Table of Contents

- [Overview](#-overview)
- [Current Features](#-current-features)
- [How It Works](#-how-it-works)
- [Technology Stack](#-technology-stack)
- [Application Routes](#-application-routes)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Environment Configuration](#-environment-configuration)
- [Available Scripts](#-available-scripts)
- [AI Output and Data Structure](#-ai-output-and-data-structure)
- [Firebase and Data Security](#-firebase-and-data-security)
- [Deployment](#-deployment)
- [Testing and Validation](#-testing-and-validation)
- [Known Limitations](#-known-limitations)
- [Documentation](#-documentation)
- [Contributing](#-contributing)
- [Security](#-security)
- [License](#-license)
- [Maintainer](#-maintainer)

## 🌍 Overview

Planning a trip often involves combining several kinds of information: destination context, traveler preferences, budget, hotel options, and a practical day-by-day itinerary. AI Trip Planner explores how a generative model can help assemble those inputs into a structured travel-plan draft.

The current application is a client-side React interface. It uses Google OAuth for sign-in, a Gemini model for itinerary generation, and Cloud Firestore for storing saved trip documents. Google Places autocomplete is used in the trip creation interface through a React integration.

The project is useful as a learning example for:
- React component composition and routing;
- handling asynchronous UI state;
- connecting a browser application to cloud services;
- creating structured prompts for generative AI;
- saving and retrieving user-specific documents;
- working with environment configuration and deployment rewrites.

It should not be treated as a production travel booking platform without the security, data validation, and operational work described in the documentation.

## ✨ Current Features

| Feature | Current implementation |
|---|---|
| Landing page | Hero section with a trip-creation entry point |
| Google sign-in | Google OAuth browser flow |
| Trip preferences | Destination, number of days, budget tier, traveler group |
| AI itinerary generation | Gemini SDK call from the browser |
| Hotel recommendations | Rendered from generated trip data |
| Day-by-day itinerary | Day sections and place cards |
| Save trips | Firestore `AITrips` collection |
| My Trips | Query trips using the stored profile email |
| Trip detail | Dynamic `/view-trip/:tripId` route |
| Toast feedback | React Hot Toast |
| UI components | Radix UI primitives with Tailwind styling |
| Deployment routing | Vercel rewrite to `index.html` |

Some provider configuration and security controls must be completed in the corresponding Google Cloud and Firebase projects. A feature shown in the UI may fail if the required external service is not configured.

## 🧭 How It Works

1. A visitor opens the landing page.
2. The visitor chooses **Create Trip**.
3. The trip form collects destination, duration, budget, and traveler group.
4. If a profile is not present in browser local storage, the app opens the Google sign-in dialog.
5. The app obtains a Google user profile and stores the profile response in local storage.
6. A prompt is built from the selected trip preferences.
7. The Gemini chat session generates itinerary text in the requested JSON format.
8. The client parses the response and writes the trip document to Firestore.
9. The app navigates to the generated trip's detail route.
10. The visitor can later open **My Trips** to retrieve saved trip documents.

**Important implementation detail:** the current browser code treats local storage as the source of the user's email when querying trips. That is not an authorization boundary. Firestore Security Rules must enforce authenticated document ownership independently of client-side filtering.

## 🧰 Technology Stack

| Area | Technologies |
|---|---|
| UI | React 18, JSX |
| Development server and build | Vite 5 |
| Routing | React Router DOM 6 |
| Styling | Tailwind CSS 3, PostCSS, Autoprefixer |
| UI primitives | Radix UI Dialog, Popover, Slot, Toast |
| Class utilities | clsx, class-variance-authority, tailwind-merge |
| Icons | Lucide React, React Icons |
| Authentication | Google OAuth React integration |
| AI | Google Generative AI SDK |
| Data persistence | Firebase Cloud Firestore |
| HTTP requests | Axios |
| Notifications | React Hot Toast |
| Hosting configuration | Vercel rewrite |

The exact dependency versions and package scripts are maintained in `package.json` and `package-lock.json`.

## 🗺️ Application Routes

| Route | Screen | Responsibility |
|---|---|---|
| `/` | Landing | Introduces the app and links to trip creation |
| `/create-trip` | Create Trip | Collects trip preferences and starts generation |
| `/my-trips` | My Trips | Lists saved trips for the locally stored profile |
| `/view-trip/:tripId` | Trip Details | Reads a Firestore document and renders the itinerary |

Routes are declared in `src/main.jsx`. The Vercel configuration rewrites application paths to the Vite entry document so the React router can resolve deep links.

## 🗂️ Project Structure

```text
AiTripPlanner/
├── public/
│   ├── logo.svg
│   ├── nirvananomads.png
│   ├── nn.png
│   └── placeholder.jpg
├── src/
│   ├── assets/
│   ├── components/
│   │   ├── custom/
│   │   │   ├── Header.jsx
│   │   │   └── Hero.jsx
│   │   └── ui/
│   │       ├── button.jsx
│   │       ├── dialog.jsx
│   │       ├── input.jsx
│   │       ├── popover.jsx
│   │       ├── toast.jsx
│   │       ├── toaster.jsx
│   │       └── use-toast.js
│   ├── constants/
│   │   └── Options.jsx
│   ├── create-trip/
│   │   └── index.jsx
│   ├── my-trips/
│   │   ├── components/
│   │   │   └── UserTripCardItem.jsx
│   │   └── index.jsx
│   ├── service/
│   │   ├── AIModal.jsx
│   │   ├── FirebaseConfig.jsx
│   │   └── GlobalApi.jsx
│   ├── view-trip/
│   │   └── [tripId]/
│   │       ├── Index.jsx
│   │       └── components/
│   │           ├── Footer.jsx
│   │           ├── Hotels.jsx
│   │           ├── InfoSection.jsx
│   │           ├── PlaceCardItem.jsx
│   │           └── PlacesToVisit.jsx
│   ├── App.css
│   ├── App.jsx
│   ├── index.css
│   ├── lib/
│   │   └── utils.js
│   └── main.jsx
├── docs/
├── .env.example
├── .editorconfig
├── components.json
├── index.html
├── jsconfig.json
├── package-lock.json
├── package.json
├── postcss.config.js
├── tailwind.config.js
├── vercel.json
└── vite.config.js
```

Generated dependencies belong in the local `node_modules/` directory and should not be committed. The npm lockfile is committed for reproducible installation.

## 🚀 Getting Started

### 1. Prerequisites

- Node.js compatible with the Vite version used by this repository.
- npm.
- Git.
- A modern browser.
- Google Cloud and Firebase project configuration for provider-backed features.

This repository uses **npm**. Do not use Bun or Yarn for the documented workflow.

### 2. Clone

```bash
git clone https://github.com/Brajesh31/AiTripPlanner.git
cd AiTripPlanner
```

### 3. Install dependencies

```bash
npm ci
```

### 4. Create local environment file

PowerShell:
```powershell
Copy-Item .env.example .env
```

macOS / Linux:
```bash
cp .env.example .env
```

Add the appropriate development values in `.env`. Do not commit this populated file.

### 5. Start development server

```bash
npm run dev
```

Open the local URL printed by Vite, normally `http://localhost:5173`.

### 6. Build for production

```bash
npm run lint
npm run build
npm run preview
```

See [Installation Guide](docs/installation.md) for detailed setup, environment, provider, and troubleshooting instructions.

## 🔐 Environment Configuration

The current `.env.example` includes:

| Variable | Purpose | Important note |
|---|---|---|
| `VITE_GOOGLE_AUTH_CLIENT_ID` | OAuth browser client ID | Configure authorized origins |
| `VITE_GOOGLE_GEMINI_AI_API_KEY` | Gemini client SDK key | Public in the built frontend, not suitable for a privileged secret |
| `VITE_GOOGLE_PLACES_API_KEY` | Places integration key | Restrict by allowed websites and enabled APIs |

Vite exposes all `VITE_*` variables in client-side code. Never treat them as confidential server secrets. The current Firebase configuration is hard-coded in `src/service/FirebaseConfig.jsx`; document and configure the Firebase project carefully.

Production recommendations:
- move Gemini calls behind a server-side endpoint;
- keep privileged credentials in the hosting platform's secret manager;
- restrict browser API keys by HTTP referrer and API;
- use Firestore Security Rules to require authenticated ownership;
- never rely on local-storage email values for access control;
- rotate any exposed key if it has been used outside its intended restrictions.

Read [Environment Configuration](docs/environment-configuration.md) and [Security](docs/security.md).

## 📜 Available Scripts

| Command | Purpose |
|---|---|
| `npm ci` | Install exact dependency tree from lockfile |
| `npm run dev` | Start Vite development server |
| `npm run lint` | Run ESLint with zero warning tolerance |
| `npm run build` | Generate production build in `dist/` |
| `npm run preview` | Preview the generated production build locally |

## 🤖 AI Output and Data Structure

The Gemini prompt requests a JSON object containing:
- trip details;
- hotel options;
- hotel names, addresses, prices, image URLs, coordinates, ratings, and descriptions;
- day-wise itinerary entries;
- place names, descriptions, image URLs, coordinates, ticket pricing, and travel-time estimates.

The UI currently expects fields such as `hotel_options`, `itinerary`, `best_time_to_visit`, `places`, `HotelName`, `HotelAddress`, `Price`, and `Rating`.

Model responses are not guaranteed to match this shape. The current implementation attempts JSON parsing and handles parse errors in the trip creation flow, but it does not provide a complete schema-validation layer. AI-generated information may be inaccurate, stale, or fabricated. Validate travel-critical information independently.

## 🔥 Firebase and Data Security

The application stores generated trip documents in the Firestore `AITrips` collection. The current client writes fields including:
- `id`;
- `userEmail`;
- `userSelection`;
- `tripData`.

The My Trips screen queries documents by `userEmail`. This client-side query does not protect data. Configure Firestore rules so a signed-in identity can read and write only its own records. The repository currently does not include Firestore rules, indexes, or automated security-rule tests.

Do not deploy unrestricted Firestore rules. See [Database](docs/database.md), [Authentication](docs/authentication.md), and [Privacy](docs/privacy.md).

## 🚢 Deployment

The repository contains `vercel.json` with a rewrite to `/index.html` for browser-history routes. A Vite deployment must build the project and publish the `dist/` directory.

Before deployment:
- configure environment values in the hosting provider;
- verify OAuth authorized origins and redirect settings;
- restrict browser API keys;
- verify Firebase project and Firestore rules;
- test all routes directly, including a refreshed trip-detail URL;
- check provider quotas, billing, and API availability;
- confirm no secret is embedded in the production bundle.

See [Deployment](docs/deployment.md).

## 🧪 Testing and Validation

Run:

```bash
npm run lint
npm run build
```

Manual browser checks should cover:
- landing and route navigation;
- Google sign-in and sign-out behavior;
- missing and valid trip form inputs;
- Gemini success, malformed output, quota, and network failure;
- Firestore save, list, read, and permission-denied behavior;
- invalid or missing trip IDs;
- mobile and desktop layouts;
- keyboard navigation and accessible dialog behavior.

The current repository does not include a dedicated automated component, browser, or Firestore rules test suite. A successful Vite build does not verify external provider configuration.

## ⚠️ Known Limitations

- Gemini is called directly from browser code with a `VITE_*` key.
- Firebase configuration is hard-coded in the client source.
- Firestore rules and indexes are not included in the repository.
- Local storage is used to retain a Google profile response.
- My Trips filters by client-provided email, which is not an authorization boundary.
- Generated JSON is not validated against a formal runtime schema before persistence.
- Provider errors and loading states are not comprehensively handled in every flow.
- Place and hotel image display currently uses a shared local placeholder in inspected view components.
- A commented-out Places REST helper exists in `GlobalApi.jsx`; it is not an active integration.
- No booking, reservation, payment, or live availability service is implemented.

These limitations should be resolved before representing the application as production-ready.

## 📖 Documentation

The `docs/` directory contains detailed guides:

| Guide | Description |
|---|---|
| [Documentation Index](docs/README.md) | Full guide directory and project scope |
| [Installation](docs/installation.md) | Setup, npm commands, environment, and local run |
| [Architecture](docs/architecture.md) | Routes, modules, integrations, and data flow |
| [Development](docs/development.md) | Coding conventions and contribution workflow |
| [API](docs/api.md) | Google, Gemini, Places, and Firebase integrations |
| [Database](docs/database.md) | Firestore collection and current document structure |
| [Authentication](docs/authentication.md) | OAuth flow and identity boundaries |
| [AI Generation](docs/ai-generation.md) | Prompt, response handling, validation, and failure modes |
| [Data Model](docs/data-model.md) | Saved trip fields and expected itinerary structure |
| [Environment Configuration](docs/environment-configuration.md) | Environment variables and key restrictions |
| [Deployment](docs/deployment.md) | Vercel build and route rewrites |
| [Security](docs/security.md) | Keys, Firestore rules, AI output, and reporting |
| [Privacy](docs/privacy.md) | User profiles, itineraries, retention, and deletion |
| [Accessibility](docs/accessibility.md) | Keyboard, dialogs, forms, and responsive review |
| [Testing](docs/testing.md) | Lint, build, manual and planned automated tests |
| [Troubleshooting](docs/troubleshooting.md) | Common setup and provider issues |
| [Performance](docs/performance.md) | Bundle, image, and provider latency review |
| [Maintenance](docs/maintenance.md) | Dependency, workflow, and repository upkeep |
| [Release Process](docs/release-process.md) | Change review and release checklist |
| [FAQ](docs/faq.md) | Common project questions |

## 🤝 Contributing

Contributions that improve reliability, code quality, accessibility, security, documentation, and user experience are welcome. Read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

Do not include private API credentials, Firebase tokens, personal information, or real travel records in commits, issues, or screenshots.

## 🔒 Security

If you discover a security issue, follow [SECURITY.md](SECURITY.md). Do not publish active credentials or private user itineraries in a public issue.

## 📄 License

This project is distributed under the MIT License. See [LICENSE](LICENSE) for the full terms.

## 👨‍💻 Maintainer

**Brajesh Kumar**  
GitHub: [@Brajesh31](https://github.com/Brajesh31)

---

<p align="center">
  Built as an AI-assisted travel planning project. Verify real-world travel details with official providers before making decisions.
</p>

Last updated: 2026-11-16 02:57 UTC
