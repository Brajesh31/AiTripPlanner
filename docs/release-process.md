# Release and Change Management

## Release model
A release is a reviewed application state built from a known commit and deployed to the configured host. The repository does not define an automated deployment workflow. Vercel routing configuration is present, but deployment success must be verified in the hosting provider.

## Before release
- Confirm intended changes are committed and review the full diff.
- Run `npm ci`, `npm run lint`, and `npm run build`.
- Verify environment values in the correct Vercel environment.
- Confirm OAuth production origin configuration.
- Confirm browser API key restrictions.
- Confirm Firestore rules deny unauthorized access.
- Test all routes, including direct deep links.
- Test AI generation failures and malformed output.
- Review privacy and accessibility impact.
- Update docs and CHANGELOG.md.

## Versioning
If tags are adopted, use a consistent versioning approach and explain what a release represents. A tag should identify a reviewed state, not merely a successful workflow run.

## Changelog and release notes
Record meaningful additions, changes, fixes, removals, and security corrections. Explain visible changes, deployment/environment steps, validation, and known limitations. Do not claim provider capabilities unless verified in the target environment.

## Deployment verification
Confirm the expected commit was built, output directory is `dist/`, all routes load directly and after refresh, OAuth works on production domain, Gemini responses are handled, Firestore ownership is enforced, no privileged key is embedded in client assets, and browser console/network requests are clean.

## Rollback
If a release breaks routes or data access, redeploy the previous known-good build. Record affected commit and reason. If credentials were exposed or user data was accessible, rollback alone is insufficient: rotate keys and correct access rules.

## Release evidence
Record deployed commit, environment, date, deployment result, test results, known risks, and rollback reference.

See [Deployment](deployment.md), [Testing](testing.md), and [Maintenance](maintenance.md).
