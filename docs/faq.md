# ❓ Frequently Asked Questions

## 📌 What does AI Trip Planner do?
It collects destination, trip duration, budget category, and traveler group, then asks a Gemini model to generate hotel suggestions and a day-by-day itinerary. Current code can save generated trip documents in Firestore.

## 📌 Is this a booking platform?
No. The app generates suggestions. It does not reserve hotels, confirm availability, sell tickets, or process payments.

## 📌 Are hotel prices and ratings verified?
No. They are model-generated suggestions and may be inaccurate or stale. Verify details with official providers.

## 📌 Which routes exist?
The router defines `/`, `/create-trip`, `/my-trips`, and `/view-trip/:tripId`.

## 📌 How do I run it?
Use `npm ci`, configure a local `.env` from `.env.example`, and run `npm run dev`. See [Installation](installation.md).

## 📌 Does it use Firebase?
Yes. It initializes Firestore and reads/writes the `AITrips` collection. Secure rules are essential. No Firestore rules file was present in the inspected repository.

## 📌 Does it use Google sign-in?
Yes. It uses a Google OAuth browser integration and stores a profile response in local storage. Local storage is not a secure identity or authorization boundary.

## 📌 Is the Gemini key safe in the frontend?
No. Vite exposes `VITE_*` values in the browser bundle. Move privileged model access behind a server endpoint for production.

## 📌 Why can AI output fail?
Model output is probabilistic. It may not be valid JSON or may omit fields expected by the UI. Validate the response before saving and rendering.

## 📌 Why does the saved trip list appear empty?
Check profile state, Firebase project, saved document fields, and Security Rules. The current query uses local-storage email and is not an authorization boundary.

## 📌 Do I need a local database?
No. The application connects to the configured Firebase Firestore project.

## 📌 How do I deploy?
Build with `npm run build` and publish `dist/`. Configure OAuth, API restrictions, Firestore rules, and SPA rewrites.

## 📌 Does CI test the full user journey?
No. Lint and build check source quality and compilation. There is no dedicated end-to-end or Firestore rules test suite in the inspected repository.

## 📌 How do I report a vulnerability?
Use the private reporting process in root [SECURITY.md](../SECURITY.md). Do not publish credentials or user travel information.
