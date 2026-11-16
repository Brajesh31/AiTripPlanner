# Testing and Validation Strategy

## Current coverage
The repository currently provides `npm run lint` and `npm run build`. No dedicated unit-test runner, React component suite, browser automation suite, or Firestore rules test suite was present in the inspected source tree. Lint and build are not complete application testing.

## Local checks
```bash
npm ci
npm run lint
npm run build
```
The lint script uses zero warning tolerance. Fix findings rather than disabling rules broadly.

## Route testing
| Route | Review |
|---|---|
| `/` | Landing content, header, call-to-action, responsive layout |
| `/create-trip` | Inputs, validation, loading state, sign-in dialog |
| `/my-trips` | Unauthenticated behavior, loading, empty state, saved cards |
| `/view-trip/:tripId` | Valid ID, missing ID, nonexistent record, malformed record |

Test direct navigation and refresh on nested routes after deployment.

## Authentication tests
Test sign-in success, cancellation, provider failure, missing profile fields, and sign-out. Verify protected data cannot be accessed by editing local storage or changing a URL. Use a dedicated development OAuth client and synthetic test account.

## Trip creation tests
Test missing destination, missing/negative/invalid duration, boundary values, missing budget, missing traveler group, valid input, duplicate clicks while pending, provider network errors, quota/permission errors, empty response, malformed JSON, missing fields, and Firestore save failure.

The current form validation condition should be reviewed for field spelling and boolean grouping. Tests should establish explicit input bounds and prove invalid input cannot proceed.

## AI response tests
Use deterministic fixtures instead of live provider calls in every test. Cover valid JSON, fenced JSON, invalid JSON, missing hotel array, missing itinerary, wrong field casing, null values, oversized arrays, malformed URLs, and unsafe text. Validate against a schema before storage and rendering.

## Firestore tests
Use a dedicated Firebase test project. Verify owner writes, owner reads, cross-user denial, unauthenticated denial, owner identity protection, invalid document rejection, missing document behavior, and permission-error UX. No Firestore rules or rules tests were present in the inspected repository.

## Responsive and accessibility tests
Review phone, tablet, desktop, keyboard navigation, dialog focus management, form labels, input errors, screen-reader names, reduced motion, and 200% zoom.

## Provider isolation
Mock external requests for automated component tests. Keep provider failures deterministic and avoid paid API quota during each test run. Do not log access tokens or complete itineraries.

## Future automation options
Consider Vitest, React Testing Library, Playwright, Firebase Emulator Suite, schema validation tests, and accessibility checks after evaluating compatibility and maintenance overhead.

## Test evidence
Record commit SHA, Node/npm versions, commands, browser and viewport, test-account type (never credentials), provider environment, results, and known failures. A passing build does not verify OAuth, Gemini, Places, Firestore rules, or deployment settings.
