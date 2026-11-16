# 📘 🔑 Authentication and Identity

## 📌 📌 Current integration
The app uses `@react-oauth/google` through `GoogleOAuthProvider` in `src/main.jsx`. The provider receives `VITE_GOOGLE_AUTH_CLIENT_ID`. The header and trip creation form call `useGoogleLogin`.

On success, browser code receives an OAuth response containing an access token and uses Axios to request Google's User Info endpoint. The returned profile is stored in localStorage under the key `user`.

## 📌 📌 Profile retrieval
The current request is:
```text
GET https://www.googleapis.com/oauth2/v1/userinfo
Authorization: Bearer <access token>
```

Do not log the token, include it in issue reports, or expose full profile responses. Use the provider's recommended token lifecycle and avoid persisting tokens unnecessarily.

## 📌 📌 Local profile cache is not an authenticated app session
localStorage is controlled by the browser user. It can be modified, cleared, or forged. Checking for `localStorage.getItem('user')` is a UI convenience only. It must never authorize Firestore access.

The Google OAuth access token is sensitive. Do not treat a user profile object as proof that the current caller owns a saved trip.

## 📌 📌 Firebase identity alignment
The current code initializes Firestore but does not demonstrate Firebase Authentication. A Google profile obtained by this OAuth package is not automatically a Firebase Auth identity.

For production data access, use a coherent trusted identity model. A common design is Firebase Authentication with Google sign-in, then Firestore rules based on `request.auth.uid`. Another design is a trusted backend that verifies Google identity and enforces ownership. Do not secure records by matching a user-controlled email field alone.

## 📌 📌 Logout behavior
The header calls `googleLogout()`, clears localStorage, and reloads the page. Clearing local storage only clears the local cache. A complete production logout design must account for Firebase Auth sign-out if introduced, any server session, token lifecycle, and cached private data.

## 📌 📌 Route protection
Routes are currently declared without a dedicated authenticated route guard. The trip creation page opens a dialog when no local profile exists, and My Trips redirects when the profile is missing. These are UX checks, not security controls.

The trip detail route accepts `tripId`. Firestore rules must decide whether the caller can read that document. Hard-to-guess IDs are not authorization.

## 📌 📌 Failure cases
Handle user cancellation, blocked popups, origin mismatch, expired tokens, failed profile requests, missing profile fields, network interruption, logout failure, and restricted localStorage. Current code logs some errors to the console; production UI should be informative without leaking token or provider details.

## 📌 📌 Test data
Use a designated non-production account and synthetic itineraries. Never paste access tokens, profile JSON, email addresses, or private travel plans into public issues.

## 📌 📌 Production checklist
- [ ] OAuth authorized origins match deployment domains.
- [ ] Consent screen and scopes are reviewed.
- [ ] Tokens are not logged or committed.
- [ ] localStorage is not an authorization authority.
- [ ] Firebase identity is aligned with trusted authentication.
- [ ] Firestore rules enforce per-user ownership.
- [ ] Logout clears the intended app session state.
- [ ] Account and itinerary deletion behavior is defined.
