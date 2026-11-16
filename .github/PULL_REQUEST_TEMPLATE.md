## Summary
Describe the problem and the change.

## Change type
- [ ] UI / accessibility
- [ ] Trip input validation
- [ ] Gemini prompt or output handling
- [ ] Authentication / identity
- [ ] Firestore / data model / rules
- [ ] Security / privacy
- [ ] Deployment / CI
- [ ] Documentation
- [ ] Dependency maintenance

## Affected routes and services
List relevant routes, components, and provider integrations.

## Data and security review
- [ ] No credentials, tokens, or private user data are included.
- [ ] Browser-exposed Vite values are not treated as secrets.
- [ ] Firestore ownership is enforced by rules, not client-only filters.
- [ ] AI output is validated before storage/rendering, where applicable.
- [ ] User profile and itinerary data are not logged.

## Validation
- [ ] `npm ci` completed.
- [ ] `npm run lint` completed.
- [ ] `npm run build` completed.
- [ ] Affected routes were tested.
- [ ] Direct route refresh was tested.
- [ ] Loading, empty, and error states were checked.
- [ ] Keyboard and responsive behavior were reviewed.
- [ ] Provider tests used non-production accounts/data.

## Screenshots / evidence
Attach screenshots for UI changes and sanitized logs for integration changes.

## Documentation
- [ ] Relevant guides and README were updated.
- [ ] Environment examples are aligned with source.
- [ ] Known limitations and future work are identified.
