# Architecture

The Vite client renders React routes and components. Service modules integrate Google OAuth, Firebase, Places autocomplete, and Gemini. Provider failures should be explicit and user-safe.

Keep privileged provider operations behind a server boundary; browser-exposed `VITE_*` values are not secrets.
