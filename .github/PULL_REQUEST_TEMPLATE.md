## Summary

Describe the problem being solved, the approach, and the visible or data behavior change.

## Change area

- [ ] Landing page or shared header
- [ ] Trip preference form
- [ ] Google OAuth / identity
- [ ] Gemini prompt or response parsing
- [ ] Firestore data or rules
- [ ] Places integration
- [ ] Trip list / trip detail
- [ ] Accessibility or responsive UI
- [ ] Documentation
- [ ] Deployment / repository automation

## Implementation details

- Routes affected:
- Components/services affected:
- Firestore fields or schema affected:
- Environment variables or provider configuration changed:
- New or changed dependencies:

## Validation

- [ ] `npm ci` completed.
- [ ] `npm run lint` passed.
- [ ] `npm run build` passed.
- [ ] Changed routes were checked directly and after refresh.
- [ ] Loading, empty, error, and provider failure states were checked.
- [ ] Responsive layouts were reviewed.
- [ ] Keyboard navigation and focus behavior were reviewed.
- [ ] Firestore ownership rules were tested if data access changed.

## Security and privacy

- [ ] No credentials or tokens were added.
- [ ] No private profile or travel data appears in code, logs, screenshots, or fixtures.
- [ ] Browser-exposed `VITE_*` values are not treated as secrets.
- [ ] Firestore authorization is enforced independently of client filtering.
- [ ] Gemini output is validated before storage/rendering.

## Evidence and known limitations

Describe test environment and results. Include sanitized screenshots or logs when useful. List known limitations and required deployment configuration.
