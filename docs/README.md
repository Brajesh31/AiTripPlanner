# 📚 Documentation Index

This documentation set is the technical and operational reference for **AI Trip Planner**. It describes the React/Vite client, its current Google and Firebase integrations, route and persistence flows, local development, deployment, and the security limitations that must be understood before production use.

> The documentation describes the repository as currently implemented. Recommendations and future improvements are labelled as such. A documented feature is not automatically complete or production-ready.

## Start here

| Goal | Guide |
|---|---|
| Install dependencies and start locally | [Installation](installation.md) |
| Understand components, routes, and service boundaries | [Architecture](architecture.md) |
| Edit the application safely | [Development](development.md) |
| Understand Google sign-in and identity boundaries | [Authentication](authentication.md) |
| Understand Gemini prompts and response handling | [AI Generation](ai-generation.md) |
| Understand saved trip fields | [Data Model](data-model.md) |
| Configure Google and Firebase values | [Environment Configuration](environment-configuration.md) |
| Understand provider integrations | [API](api.md) |
| Understand Firestore reads, writes, and rules | [Database](database.md) |
| Prepare Vercel deployment | [Deployment](deployment.md) |
| Review security risks and mitigations | [Security](security.md) |
| Understand personal data and retention | [Privacy](privacy.md) |
| Test keyboard, forms, and dialogs | [Accessibility](accessibility.md) |
| Run validation and test scenarios | [Testing](testing.md) |
| Review bundle and provider latency | [Performance](performance.md) |
| Maintain dependencies and workflows | [Maintenance](maintenance.md) |
| Prepare and verify a release | [Release Process](release-process.md) |
| Diagnose common problems | [Troubleshooting](troubleshooting.md) |
| Find common answers | [FAQ](faq.md) |

## Current application at a glance

| Area | Current implementation |
|---|---|
| Frontend | React 18 with JSX |
| Development server and build | Vite 5 |
| Routing | React Router DOM 6 |
| Styling | Tailwind CSS 3, PostCSS, Autoprefixer |
| Components | Radix UI primitives and custom components |
| Authentication | Google OAuth browser integration |
| AI provider | Google Generative AI SDK, browser-side Gemini call |
| Persistence | Firebase Cloud Firestore client SDK |
| Places | Google Places autocomplete React package |
| Notifications | React Hot Toast |
| Deployment configuration | Vercel rewrite to the Vite entry document |
| Package manager | npm with `package-lock.json` |

## Application routes

- `/`: landing page.
- `/create-trip`: trip preference form and AI generation flow.
- `/my-trips`: saved trip list.
- `/view-trip/:tripId`: Firestore-backed trip details.

The route configuration is in `src/main.jsx`.

## Important security and reliability boundaries

- Browser variables prefixed with `VITE_` are visible in the client bundle. They are not private secrets.
- The current Gemini SDK call runs in the browser.
- Firebase web configuration is hard-coded in `src/service/FirebaseConfig.jsx`.
- The client stores a Google profile response in local storage and uses its email in a Firestore query.
- Client-side filtering is not authorization. Firestore Security Rules must enforce document ownership.
- Gemini output is untrusted text and must be validated before storage or rendering.
- An AI-generated itinerary is not verified inventory, confirmed hotel availability, or a reservation.

These limitations are documented so they can be addressed rather than hidden.

## Documentation maintenance rules

1. Keep instructions aligned with source code and package scripts.
2. Distinguish implemented behavior from future recommendations.
3. Never publish real secrets, tokens, private itineraries, or personal data.
4. When provider integrations or Firestore fields change, update related guides.
5. Use reproducible commands and identify OS-specific syntax.
6. Verify relative links after editing documentation.
7. Do not mark a system secure or production-ready without evidence.

## Related repository files

- `README.md`: project overview and quick start.
- `CONTRIBUTING.md`: contribution workflow.
- `SECURITY.md`: vulnerability reporting.
- `SUPPORT.md`: support guidance.
- `CODE_OF_CONDUCT.md`: conduct expectations.
- `CHANGELOG.md`: repository change history.
- `ROADMAP.md`: planned engineering improvements.
- `CITATION.cff`: software citation metadata.
