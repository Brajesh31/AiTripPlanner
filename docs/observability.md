# 🔎 Diagnostics and Observability

This guide describes diagnosis for the current browser-based application and recommends production observability. It does not claim a centralized logging or metrics platform is installed.

## Current diagnostic tools

- Browser developer tools and console.
- Network inspection for Google OAuth, Gemini, and Firebase requests.
- Vite development server output.
- ESLint and production build.
- GitHub Actions logs for lint/build jobs.
- Firebase console for Firestore investigation.

Some current code logs prompts, profile data, and model response content. Remove sensitive logging before production.

## Diagnostic sequence

1. Reproduce using a test account and non-sensitive destination.
2. Record route, browser, approximate timestamp, and user action.
3. Inspect console exceptions.
4. Inspect request status and provider error category.
5. Check the provider dashboard without copying tokens into an issue.
6. Identify the failing stage: sign-in, generation, parsing, persistence, or rendering.
7. Retry with a minimal reproducible request.
8. Capture sanitized evidence and add a regression test when test infrastructure exists.

## Failure domains

| Domain | Symptoms | First checks |
|---|---|---|
| Vite/build | Blank page or failed build | Node version, npm install, console, build logs |
| OAuth | Sign-in denied or profile request fails | Client ID, authorized origin, consent config, scopes |
| Gemini | 403, quota, timeout, empty text | Key restriction, API enablement, quota, model config |
| JSON parsing | Generation returns but save fails | Response shape, code fences, truncation, parser error |
| Firestore | Permission denied or unavailable | Project, network, Rules, collection name |
| Route | Detail page fails on refresh | Router definition, host SPA fallback |
| Assets | Image does not display | URL validity, hotlink policy, network response |

## Production telemetry recommendations

Add telemetry only after deciding what data is necessary and how long it is retained.

Useful measurements:
- trip-generation request count and latency;
- generation success/failure rate by error category;
- model response validation failure rate;
- Firestore read/write failure rate;
- route-level client errors;
- application build/deployment version.

Never send OAuth tokens, API keys, personal prompts, raw profile objects, or full itineraries unless explicitly necessary and governed by a privacy policy.

## Example structured event

A future event could use this shape:

```json
{
  "event": "trip_generation_failed",
  "requestId": "opaque-correlation-id",
  "stage": "model_response_validation",
  "category": "invalid_output",
  "appVersion": "release-tag",
  "timestamp": "ISO-8601"
}
```

Use opaque correlation IDs. Avoid email, destination, or free-form prompts as metric labels.

## Incident response

- Revoke or rotate credentials involved in an exposure.
- Restrict provider usage and review billing.
- Investigate database permissions before resuming user-data writes.
- Preserve sanitized workflow and browser errors.
- Communicate scope and impact accurately.
- Record remediation and prevention steps.

Related: [Security](security.md), [Privacy](privacy.md), [Troubleshooting](troubleshooting.md), [Release Checklist](release-checklist.md).
