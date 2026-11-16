# Repository Maintenance Guide

## Routine maintenance
Review whether README commands work, docs match source, links resolve, package scripts remain accurate, dependencies are maintained, keys remain restricted, Firestore rules protect user records, issue templates remain useful, and workflows have suitable permissions.

## Dependency management
The repository uses npm and `package-lock.json`. Install with `npm ci`. Do not commit `node_modules/`. Use Dependabot or reviewed manual updates for packages and GitHub Actions. Review changelogs and breaking changes before updating React, Vite, Firebase, OAuth, or the Google AI SDK.

Do not run `npm audit fix --force` blindly. Review the vulnerability, dependency path, available patch, and breaking-change risk first.

## Provider configuration
Periodically review OAuth authorized origins, browser API-key restrictions, enabled APIs, billing and quota, Firebase project settings, and Firestore Security Rules. Rotate keys if exposure or unauthorized use is suspected.

## GitHub Actions
Use minimum workflow permissions. Do not expose secrets to untrusted pull-request code. Review action versions and logs. A passing workflow verifies only the checks it actually performs.

## File hygiene
Do not commit OS metadata, editor caches, build output, temporary logs, local environment files, or generated dependency directories. Preserve the npm lockfile. Keep IDE configuration only when intentionally shared and useful.

## Documentation maintenance
Route changes require updates to README, architecture, testing, and deployment guides. AI fields require updates to AI generation and data model guides. OAuth changes affect authentication, privacy, and security. Firestore schema/rules changes affect database, data model, security, and testing. Environment changes affect the example, installation, deployment, and config guide. Build changes affect installation and CI.

## Incident response
For leaked keys, revoke or rotate and inspect provider usage. For exposed travel data, review Firestore access and logs. For broken deployments, redeploy the last known-good build. Follow private reporting in SECURITY.md.

## Checklist
- [ ] README and docs reflect current code.
- [ ] Lint and production build pass.
- [ ] npm lockfile is synchronized with package.json.
- [ ] node_modules and local environment files are not tracked.
- [ ] Provider keys are restricted.
- [ ] Firestore access rules are reviewed.
- [ ] User-data retention and deletion are documented.
- [ ] Workflows use minimum permissions.
