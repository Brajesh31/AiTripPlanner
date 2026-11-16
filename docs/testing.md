# 📘 🧪 Testing and Validation Strategy

## 📌 📌 Current test tooling
The repository currently defines `npm run lint` and `npm run build`. The reviewed source tree does not contain a dedicated unit, integration, or browser automation suite. A successful Vite build confirms compilation, not correct authentication, provider behavior, Firestore authorization, or accessible interactions.

## 📌 📌 Baseline validation
Run:
```bash
npm ci
npm run lint
npm run build
npm run preview
```
Record Node and npm versions, commit SHA, command output, and any known lint exceptions. Do not claim a command passed unless it was run successfully.

## 📌 📌 Route smoke tests
Verify that `/` loads the landing page and header, `/create-trip` renders the preference form, `/view-trip/<synthetic-id>` loads a known test document or gives a clear not-found state, and `/my-trips` behaves correctly both signed out and signed in. Direct navigation and browser refresh must work on every route.

## 📌 📌 Form tests
Check empty destination, missing duration, zero/negative duration, non-integer duration, missing budget, missing traveler group, excessive destination text, and rapid repeated submission. Confirm that numeric duration state is actually stored as a number.

## 📌 📌 OAuth tests
Use a dedicated test account. Test success, cancellation, blocked popup, incorrect authorized origin, User Info network failure, missing profile fields, and logout. Never use a real personal access token in test evidence.

## 📌 📌 Gemini tests
Use synthetic destinations and a restricted non-production key. Test valid JSON, malformed JSON, empty output, fenced JSON, missing hotel fields, missing itinerary, missing places array, rate limit, quota exhaustion, timeout, and provider unavailability. Validate output before Firestore writes. Keep generated content clearly marked as suggestions.

## 📌 📌 Firestore tests
Use a non-production Firebase project or Emulator. Test create, list, detail, unauthorized read, spoofed email, cross-user document access, missing fields, malformed data, and deletion behavior when implemented. Client-side filters must not be treated as access control.

## 📌 📌 Accessibility tests
Use keyboard-only navigation, visible focus review, form label inspection, dialog focus management, screen reader review, 200% zoom, reduced motion, and small viewport checks. See [Accessibility](accessibility.md).

## 📌 📌 Regression checklist
- [ ] Lint command completes.
- [ ] Production build completes.
- [ ] All four routes render.
- [ ] Direct route refresh works.
- [ ] Form validation behaves correctly.
- [ ] OAuth errors are handled safely.
- [ ] Gemini malformed output is rejected.
- [ ] Firestore rules prevent cross-user access.
- [ ] Loading, empty, and failure states are understandable.
- [ ] No secrets or private itineraries appear in logs.
- [ ] Keyboard and responsive behavior are checked.

## 📌 📌 Future automation
Consider unit tests for input normalization and response validation, component tests for form and empty states, Firestore Emulator rules tests, and Playwright browser smoke tests. Add frameworks only with documented setup, scripts, and maintainable coverage goals.
