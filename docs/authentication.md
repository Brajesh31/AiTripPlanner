# Authentication and User Identity

## Current implementation

The application uses the Google OAuth React integration. The OAuth provider is mounted in `src/main.jsx` using `VITE_GOOGLE_AUTH_CLIENT_ID`. The header and trip-creation screen use the OAuth hook to start sign-in.

After OAuth returns an access token, the current client requests user profile information from Google's user-info endpoint. It then stores the returned profile object in browser local storage under the key `user`. The UI reads that value to decide whether to show sign-in controls and to obtain the user's email for trip queries.

## Current sign-in flow

1. Visitor opens the application.
2. The header checks for a `user` item in local storage.
3. When a visitor attempts to create a trip without that item, the sign-in dialog is opened.
4. The user selects Google sign-in.
5. The browser OAuth SDK obtains an access token.
6. The app requests profile information from Google.
7. The response is stored in local storage.
8. The dialog closes and trip generation continues.

The current implementation is a lightweight client-side sign-in flow. It is not a complete secure session system and should not be described as one.

## Identity and authorization limitations

Local storage can be modified by the browser user and must never be trusted as proof of identity. A stored email value is not enough to grant access to a Firestore document.

The My Trips screen currently queries Firestore using the profile email string. A user can alter that value or attempt to read a trip document directly by ID. Access control must be enforced by Firestore Security Rules or a trusted backend.

## Production recommendation

Use Firebase Authentication with Google as a provider, or validate Google identity tokens on a trusted server. Ensure Firestore rules use the authenticated identity to enforce ownership. Avoid authorizing requests using profile fields supplied by the client.

A production identity design should define:
- provider and OAuth client configuration;
- authorized JavaScript origins and redirect behavior;
- token validation and lifecycle;
- sign-in cancellation and error handling;
- sign-out and local data cleanup;
- session persistence and expiration;
- user account linking and duplicate accounts;
- Firestore ownership enforcement;
- account deletion and data retention.

## Local storage

The current app stores profile data in local storage. Treat it as untrusted display state:
- do not store access tokens unless there is a deliberate secure design;
- do not store unnecessary profile fields;
- handle malformed or missing JSON safely;
- clear the app-owned key on logout rather than deleting unrelated origin storage;
- do not log profile objects;
- expect local storage to be unavailable in some browser privacy modes.

## Sign-out behavior

The header invokes the Google logout helper, clears local storage, and reloads the page. Review this behavior before production. A robust implementation should sign out from the actual application authentication provider, clear only application-owned state, and verify that protected views no longer render.

## Testing checklist

- [ ] OAuth sign-in succeeds on configured origins.
- [ ] User cancellation is handled without a broken state.
- [ ] Provider errors show a useful message.
- [ ] Missing or malformed local profile does not crash the app.
- [ ] Sign-out clears application authentication state.
- [ ] Direct route access without authentication is handled.
- [ ] Firestore denies unauthenticated reads and writes.
- [ ] One user cannot access another user's trip by changing local storage or URL.
- [ ] No access token or private profile fields appear in logs.

See [Database](database.md), [Privacy](privacy.md), and [Security](security.md).
