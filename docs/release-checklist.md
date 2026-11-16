# 🚦 Release Readiness Checklist

Use this before tagging or deploying. A successful Vite build alone is not release approval.

## Source and dependencies

- [ ] Confirm intended branch and commit.
- [ ] Review changed files; remove accidental generated output.
- [ ] Confirm lockfile matches `package.json`.
- [ ] Install with `npm ci`.
- [ ] Review dependency updates and advisories.
- [ ] Confirm `node_modules/`, `.env`, and build output are not tracked.

## Automated checks

- [ ] `npm run lint` passes.
- [ ] `npm run build` passes.
- [ ] GitHub Actions CI passes on the exact commit.
- [ ] No secrets appear in source, diffs, build output, or logs.

## Functional checks

- [ ] Landing route loads.
- [ ] Create-trip form accepts valid preferences.
- [ ] Required-field validation is correct.
- [ ] OAuth success, cancellation, and failure are checked.
- [ ] Gemini request failure and empty response are handled.
- [ ] Unexpected model output is rejected safely.
- [ ] Firestore write success and permission denial are checked.
- [ ] Saved-trip list handles empty, loading, and error states.
- [ ] Valid and invalid detail IDs behave correctly.
- [ ] Browser refresh works on every client route.

## Security gate

- [ ] No privileged Gemini key is bundled into client JavaScript.
- [ ] AI generation uses a trusted server-side endpoint.
- [ ] User identity is verified server-side or through Firebase Authentication.
- [ ] Firestore Rules enforce ownership by immutable UID.
- [ ] Rules are tested for authorized and unauthorized users.
- [ ] No tokens or personal trip data are written to logs.
- [ ] Model output is schema-validated and bounded before persistence/rendering.
- [ ] Privacy and deletion expectations are documented.

**Do not mark this gate complete based only on environment variable names or client-side checks.** The current application has browser-side AI access and localStorage-based profile handling.

## Accessibility and responsive review

- [ ] Keyboard-only navigation works.
- [ ] Dialog focus is managed and restored.
- [ ] Form labels and errors are announced.
- [ ] Focus visibility is clear.
- [ ] Layout works at narrow viewport widths.
- [ ] Images have useful alt text where informative.
- [ ] Contrast is adequate.

## Deployment

- [ ] Hosting environment is configured.
- [ ] Host serves the `dist/` output.
- [ ] SPA fallback serves `index.html` for nested routes.
- [ ] OAuth origins match deployment domain.
- [ ] Firebase domain and Rules configuration match deployment.
- [ ] Deployed smoke test is complete.
- [ ] Rollback target and owner are known.

## Release record

| Field | Value |
|---|---|
| Version / tag | |
| Commit SHA | |
| Release owner | |
| CI run URL | |
| Deployment URL | |
| Manual test evidence | |
| Known limitations | |
| Rollback reference | |

## Stop-ship conditions

Do not release with an exposed privileged API key, unrestricted or untested database access, failed CI, broken primary trip flow, cross-user data exposure, or unvalidated model output used as trusted data.
