# 📘 Project Glossary

| Term | Meaning |
|---|---|
| AI itinerary | Model-generated suggestions for hotels and places grouped by trip day |
| Budget category | Selected budget preference supplied to the prompt |
| Traveler category | Selected party type supplied to the prompt |
| Trip preferences | Destination, day count, budget, and traveler category |
| Gemini | Google's generative AI service used by the current generation module |
| Prompt | Text template populated with trip preferences |
| Model output | Untrusted text expected to contain JSON |
| JSON parsing | Converting response text into a JavaScript value; not schema validation |
| Schema validation | Checking required fields, types, limits, and nested structures |
| Firestore | Firebase cloud document database |
| Collection | Logical group of Firestore documents, currently `AITrips` |
| Document | A stored record identified by a document ID |
| localStorage | Browser storage used to cache a Google profile in the current app |
| OAuth | Protocol used by the Google sign-in flow |
| Access token | Identity-provider credential that must be handled as sensitive |
| Vite environment variable | Build-time value; `VITE_*` variables are exposed to client code |
| SPA | Single-page application where client-side routing handles views |
| SPA fallback | Hosting rule that serves `index.html` for client-side routes |
| CI | Automated checks on repository changes |
| Build artifact | Static output from `npm run build`, located in `dist/` |
| Least privilege | Granting each identity only the access needed for its task |
| Trust boundary | Boundary between components with different security assumptions |
| Idempotency | Repeating an operation without unintended duplicate effects |
| Rate limiting | Restricting request frequency to reduce abuse and unexpected cost |
| Data retention | How long saved user data is kept before deletion or archival |

## Project naming

- Application: **AI Trip Planner**
- Repository: `AiTripPlanner`
- Firestore collection: `AITrips`
- Trip detail route: `/view-trip/:tripId`
- Build output: `dist/`
- Local environment file: `.env`, never commit real values
