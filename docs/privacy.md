# 🛡️ Privacy and Data Lifecycle

## 📌 Scope
AI Trip Planner collects trip preferences, traveler category, Google profile information, and generated itinerary content in its current flow. It sends prompt inputs to Gemini, uses Google OAuth profile retrieval, and stores trip documents in Firestore. This is a technical data-handling guide, not legal advice or a claim of compliance with every applicable law.

## 📌 Data visible in source
| Data | Current handling |
|---|---|
| Destination | Entered in form and included in Gemini prompt |
| Trip duration | Entered by user and stored in preference object |
| Budget | Included in prompt and stored |
| Traveler group | Included in prompt and stored |
| Google profile | User Info response stored in localStorage |
| User email | Written as `userEmail` in Firestore and used by My Trips query |
| Generated itinerary | Parsed from Gemini and stored in Firestore |
| OAuth access token | Used for Google User Info request; must not be logged |

The current code does not demonstrate a complete consent flow for AI processing or a data-deletion feature.

## 📌 Provider processing
Trip preference fields are interpolated into a Gemini prompt. OAuth access tokens are sent to Google's User Info endpoint. Generated trip documents are stored in the configured Firebase project. Review the terms, retention, processing location, and account configuration for the actual provider projects.

## 📌 Data minimization
Collect only what is needed to generate and display an itinerary. Do not request passport numbers, payment details, identity documents, home addresses, or unnecessary sensitive information. A traveler category is a planning preference, not a reason to collect individual identities.

Avoid logging full prompts, OAuth responses, profile objects, or saved itineraries. Use synthetic records for tests and issue reports.

## 📌 localStorage
The application stores a Google profile object under `user`. localStorage is available to same-origin JavaScript and is user-modifiable. It should not contain long-lived secrets and must not be used as trusted identity for authorization.

## 📌 Firestore
Trip documents include email and itinerary content. Rules must enforce ownership; a client-side query filter is not authorization. The inspected repository does not contain a Firestore rules file, so deployers must not assume the database is protected.

See [Firestore Data Model](firestore-data-model.md) and [Authentication](authentication.md).

## 📌 Retention and deletion
The current UI does not document individual trip deletion or account deletion. Before public use, define how users view and delete trips, request account removal, delete associated Firestore records, handle backups, and verify requests.

## 📌 User-facing transparency
Explain that trip preferences are sent to an AI provider, that saved trip data is stored in Firebase, and that generated information must be verified. Provide understandable sign-out and data-management options.

## 📌 Security incidents
If a key or token is exposed, revoke or rotate it. If itinerary records are exposed, investigate access, affected records, and provider logs. Do not publish private itinerary contents in issues.

## 📌 Checklist
- [ ] Data collection purpose is explained.
- [ ] AI processing and provider use are disclosed.
- [ ] Only necessary fields are collected.
- [ ] Sensitive tokens and full profiles are not logged.
- [ ] Firestore rules enforce record ownership.
- [ ] localStorage is not an authorization mechanism.
- [ ] Retention and deletion processes are defined.
- [ ] Test fixtures and issue evidence use synthetic data.
