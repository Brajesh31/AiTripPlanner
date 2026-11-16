<div align="center">\n\n# 🌍 AI Trip Planner\n\n### ✈️ Plan smarter. Explore further. Travel with an AI-generated itinerary.\n\n![React](https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=black) ![Vite](https://img.shields.io/badge/Vite-5-646CFF?logo=vite&logoColor=white) ![Firebase](https://img.shields.io/badge/Firebase-Firestore-FFCA28?logo=firebase&logoColor=black) ![Gemini](https://img.shields.io/badge/AI-Gemini-4285F4?logo=google&logoColor=white) ![License](https://img.shields.io/badge/License-MIT-green)\n\n**An AI-assisted travel itinerary planner built with React, Vite, Gemini, and Firebase.**\n\n[📚 Documentation](docs/README.md) · [🐛 Report a Bug](https://github.com/Brajesh31/AiTripPlanner/issues/new?template=bug_report.md) · [💡 Request a Feature](https://github.com/Brajesh31/AiTripPlanner/issues/new?template=feature_request.md)\n\n</div>

AI Trip Planner is a React and Vite application that helps users create AI-generated travel itineraries from a destination, trip duration, budget category, and traveler group. The application uses Google sign-in, Gemini-generated suggestions, and Firebase Cloud Firestore to save and retrieve trip records.

> ⚠️ **Important:** This is an itinerary suggestion tool, not a booking platform. AI-generated hotels, addresses, prices, ratings, images, opening hours, travel times, ticket costs, and safety details may be inaccurate or outdated. Verify important information with official providers. The application does not make reservations or guarantee availability.

## ✨ What the application currently does

| Capability | Current implementation |
|---|---|
| Landing page | React hero page |
| Trip preferences | Destination, number of days, budget, traveler category |
| Sign-in | Google OAuth browser flow |
| AI itinerary | Gemini SDK generates expected JSON response |
| Trip persistence | Firestore `AITrips` collection |
| My Trips | Queries saved documents using cached profile email |
| Trip details | Loads one Firestore document by route ID |
| Hotel suggestions | Displays generated fields and Google Maps search links |
| Places by day | Renders itinerary groups and place cards |
| Places autocomplete | Package imported, but current field is a normal text input |
| First-party backend API | Not implemented |

## 🧰 Technology stack

- **Frontend:** React 18 and JSX
- **Build:** Vite 5
- **Routing:** React Router DOM 6
- **Styling:** Tailwind CSS 3 and PostCSS
- **UI primitives:** Radix UI and class-variance-authority
- **Authentication:** `@react-oauth/google`
- **AI:** `@google/generative-ai`, configured for `gemini-1.5-flash`
- **Database:** Firebase Cloud Firestore
- **HTTP:** Axios
- **Icons and feedback:** Lucide React, React Icons, React Hot Toast
- **Hosting configuration:** Vercel SPA rewrite

## 🚀 Getting started

### Prerequisites

Install Git, Node.js compatible with the Vite 5 toolchain, npm, and a modern browser.

### 1. Clone the repository

```bash
git clone https://github.com/Brajesh31/AiTripPlanner.git
cd AiTripPlanner
```

### 2. Install dependencies

```bash
npm ci
```

Use the committed lockfile for reproducible installation. Do not commit a local `node_modules/` directory.

### 3. Configure environment variables

Copy the example file.

**PowerShell**
```powershell
Copy-Item .env.example .env
```

**macOS / Linux**
```bash
cp .env.example .env
```

Set the values described in [Environment Configuration](docs/environment.md):

```dotenv
VITE_GOOGLE_AUTH_CLIENT_ID=your-browser-oauth-client-id
VITE_GOOGLE_GEMINI_AI_API_KEY=your-restricted-development-key
VITE_GOOGLE_PLACES_API_KEY=your-restricted-browser-key
```

**Security warning:** Vite exposes all `VITE_*` values in the browser bundle. The current Gemini integration uses a browser-visible key. Do not use a privileged key here. Move AI generation to a trusted server-side function before production. Firebase web configuration is hard-coded in the current service module and must be protected with correctly configured Firestore Security Rules and provider restrictions.

### 4. Start development

```bash
npm run dev
```

Open the local URL printed by Vite, commonly `http://localhost:5173`.

## 📜 Available scripts

| Command | Purpose |
|---|---|
| `npm run dev` | Start the Vite development server |
| `npm run build` | Build optimized output into `dist/` |
| `npm run preview` | Preview the production build locally |
| `npm run lint` | Run ESLint with the current repository script |

Before submitting changes:

```bash
npm run lint
npm run build
```

## 🧭 Application routes

| Route | Component | Description |
|---|---|---|
| `/` | `App` / `Hero` | Landing page |
| `/create-trip` | `CreateTrip` | Collect preferences and generate a trip |
| `/view-trip/:tripId` | `ViewTrip` | Read and render one saved trip |
| `/my-trips` | `MyTrips` | List saved trips for the cached profile |

## 🗂️ Project structure

```text
AiTripPlanner/
├── public/                         # Public images and static assets
├── src/
│   ├── components/
│   │   ├── custom/                 # Header and Hero
│   │   └── ui/                     # Button, dialog, input, popover, toast
│   ├── constants/                  # Budget, traveler, and prompt options
│   ├── create-trip/                 # Preference and generation flow
│   ├── my-trips/                    # Saved-trip listing
│   ├── service/                     # Gemini, Firebase, provider helpers
│   ├── view-trip/[tripId]/          # Trip detail page and components
│   ├── App.jsx                      # Landing component
│   ├── main.jsx                     # Entry, providers, and routes
│   ├── App.css
│   └── index.css
├── docs/                            # Technical and operational guides
├── .github/                         # Workflow and PR guidance
├── .env.example                     # Placeholder browser configuration
├── package.json
├── package-lock.json
├── vite.config.js
├── tailwind.config.js
├── postcss.config.js
├── components.json
├── jsconfig.json
├── vercel.json
└── index.html
```

## 🔄 How trip generation works

1. The user opens Create Trip and enters destination, duration, budget, and traveler category.
2. The app checks for a cached Google profile in localStorage.
3. If no profile exists, the user can sign in with Google OAuth.
4. The app requests profile data from Google's User Info endpoint.
5. User input is interpolated into the AI prompt.
6. The browser calls Gemini and receives text expected to contain JSON.
7. The app parses the response and writes user selection, trip data, email, and ID to Firestore.
8. The browser navigates to `/view-trip/:tripId`.
9. The detail page reads the document and renders hotel and itinerary sections.
10. My Trips queries records filtered by the cached profile email.

This describes the current client implementation. It is not a production-grade identity or authorization design.

## 🚨 Security and deployment warnings

The repository has important production hardening requirements:

- The Gemini API key is read through a `VITE_*` variable and used in browser code. It is exposed to visitors. Move privileged AI requests behind a server-side function and rotate keys that have escaped their intended scope.
- Firebase web configuration is hard-coded in `src/service/FirebaseConfig.jsx`. Firebase web config is not a substitute for Firestore Security Rules.
- The app stores a Google profile object in localStorage. Client-side state can be forged and must not authorize Firestore reads or writes.
- The Google OAuth profile flow is not demonstrated as a Firebase Authentication session. Align identity before enforcing Firestore ownership by UID.
- The reviewed repository tree does not contain a Firestore rules file. Do not use real user data until access rules are created and tested.
- Gemini output is parsed with `JSON.parse` but is not fully schema-validated before persistence.
- Generated trip data is probabilistic. Verify hotel names, addresses, prices, opening times, transport, and attraction information independently.

Read [Security](docs/security.md), [Authentication](docs/authentication.md), [Firestore Data Model](docs/firestore-data-model.md), and [AI Output Contract](docs/ai-output-contract.md).

## 🧪 Testing and validation

The project includes ESLint and a Vite production build. A dedicated unit, integration, or browser automation suite was not present in the source tree reviewed for this documentation.

```bash
npm ci
npm run lint
npm run build
npm run preview
```

Manually check all routes, OAuth behavior, Firestore permissions, generation errors, malformed AI output, mobile layouts, keyboard navigation, and direct route refresh. See [Testing](docs/testing.md).

## 📚 Documentation

Start with [docs/README.md](docs/README.md).

| Guide | Focus |
|---|---|
| [Installation](docs/installation.md) | Local setup and scripts |
| [Architecture](docs/architecture.md) | Components, routes, and service boundaries |
| [Development](docs/development.md) | Code structure and workflow |
| [API and Services](docs/api.md) | Gemini, OAuth, Firebase, and Places |
| [Database](docs/database.md) | Firestore collection and document behavior |
| [Environment](docs/environment.md) | Vite variables and provider restrictions |
| [AI Output Contract](docs/ai-output-contract.md) | Model prompt and expected JSON |
| [Authentication](docs/authentication.md) | Google OAuth and identity limitations |
| [Firestore Data Model](docs/firestore-data-model.md) | Data ownership and rules expectations |
| [Privacy](docs/privacy.md) | Profile and trip data lifecycle |
| [Security](docs/security.md) | Key exposure and production risks |
| [Deployment](docs/deployment.md) | Build and hosting |
| [Deployment Operations](docs/deployment-operations.md) | Vercel environment, route fallback, rollback |
| [Accessibility](docs/accessibility.md) | Forms, dialogs, focus, generated content |
| [Performance](docs/performance.md) | Bundles, SDKs, images, latency |
| [Testing](docs/testing.md) | Automated and manual test strategy |
| [Maintenance](docs/maintenance.md) | Dependency, configuration, and repository upkeep |
| [Troubleshooting](docs/troubleshooting.md) | Common local and provider issues |
| [FAQ](docs/faq.md) | Common questions |

## 🛣️ Roadmap

See [ROADMAP.md](ROADMAP.md) for planned architecture, security, test, and product improvements. Planned work is not current functionality.

## 🤝 Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Contributions should include lint/build validation, route testing, security review for provider changes, and documentation.

## 📄 License

Released under the [MIT License](LICENSE).

## 👨‍💻 Maintainer

**Brajesh Kumar**  
GitHub: [@Brajesh31](https://github.com/Brajesh31)

---

*AI-generated travel suggestions are not verified travel advice. Confirm important details with official sources before making travel decisions.*
