# 🧹 Repository Maintenance

## 📌 Purpose
Maintain the React/Vite application without adding unnecessary infrastructure or allowing generated files, stale documentation, or exposed credentials to accumulate.

## 📌 Routine checks
Periodically review the README, environment example, package manifest and lockfile consistency, route configuration, service modules, provider model versions, Firebase Security Rules, deployment rewrite, CI configuration, issue templates, media rights, and roadmap.

## 📌 Dependency maintenance
Use npm and the committed `package-lock.json`. For an update, inspect package release and migration notes, update the manifest deliberately, run npm install to update the lockfile, review the lockfile diff, run lint and production build, and test affected provider and UI flows. Commit manifest and lockfile together.

Do not manually edit generated dependency files. The current repository has a large tracked `node_modules/` tree. Generated dependencies should be removed from version control in a deliberate cleanup while keeping the lockfile. Do not remove partial directory fragments and claim cleanup is complete.

## 📌 Workflow maintenance
The repository currently includes README timestamp automation but no complete application CI pipeline. A future CI workflow should use npm ci, lint, build, least-privilege permissions, and concurrency controls. It should not deploy unless deployment is intentionally configured.

## 📌 Configuration maintenance
Keep `.env.example` aligned with actual `import.meta.env` references. Never add populated secrets. Document whether each variable is public browser configuration or server-only. Review provider restrictions and quotas.

## 📌 Data maintenance
Review Firestore indexes, Security Rules, backup policy, retention, deletion behavior, and development/production separation. Never export real user itineraries into public fixtures.

## 📌 Documentation maintenance
When source behavior changes, update architecture, API, environment, authentication, Firestore, privacy, security, testing, and deployment documents as applicable. Remove claims that no longer match code. Add new guides to this index and README.

## 📌 File hygiene
Ignore node_modules, dist, environment files, caches, logs, coverage, and IDE state unless a shared project setting is intentionally required. Retain package-lock.json. Review tracked artifacts before release.

## 📌 Incident response
For exposed credentials, revoke or rotate them immediately and assess provider usage. For data exposure, investigate affected records and provider logs. For a bad deployment, restore the last known-good deployment and verify all direct routes.

## 📌 Maintenance checklist
- [ ] Package manifest and lockfile are consistent.
- [ ] No generated dependency tree is tracked.
- [ ] .env and private data are not committed.
- [ ] Provider keys are restricted and rotated where needed.
- [ ] Firestore rules and indexes are reviewed.
- [ ] Lint and build checks are green.
- [ ] Routes and failure states are tested.
- [ ] Documentation reflects actual source behavior.
- [ ] Roadmap and changelog are accurate.
