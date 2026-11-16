# 🔐 Security Policy

## Project security scope

AI Trip Planner is a browser-first React application that handles Google profile information, travel preferences, AI-generated itinerary data, and saved Firestore documents. It integrates with Google OAuth, Gemini, Places, and Firebase.

This policy is not a penetration-test report or security certification. It records how to report issues and the security boundaries contributors must preserve.

## Reporting a vulnerability

Report suspected vulnerabilities privately to the maintainer, **Brajesh Kumar** ([ @Brajesh31 ](https://github.com/Brajesh31)), using GitHub's private vulnerability reporting facility when available. Do not publish exploit instructions, live keys, access tokens, private itineraries, or personal information in a public issue.

Include:
- affected route, file, workflow, or provider integration;
- concise issue description and realistic impact;
- sanitized reproduction steps;
- browser/runtime and commit details;
- relevant sanitized logs or screenshots;
- suggested mitigation, if known.

## Response process

The maintainer will review the report, attempt reproduction, assess impact, and determine the appropriate remediation. Response time depends on availability and severity; no formal service-level agreement is promised. Please allow a reasonable opportunity to investigate a confirmed issue before public disclosure.

## Known security concerns requiring attention

The inspected source currently:
- calls Gemini directly from the browser using a `VITE_*` key;
- stores a Google profile response in local storage;
- uses the stored email as a Firestore query condition;
- initializes Firebase from hard-coded browser configuration;
- parses model-generated JSON without a complete runtime schema validation layer;
- contains no Firestore rules file in the repository tree.

These are material production risks. Local storage is not an identity proof, a client-side query is not authorization, and a `VITE_*` value is not secret.

## Contributor requirements

- Never commit keys, tokens, passwords, private keys, or production credentials.
- Never put privileged secrets in client-side `VITE_*` values.
- Use Firestore rules to enforce authenticated ownership.
- Validate model output and user input before persistence or rendering.
- Do not log tokens, profile objects, private prompts, or full itineraries.
- Use synthetic user data in examples and tests.
- Restrict browser API keys by origin and allowed API.
- Do not expose secrets to untrusted pull-request code.
- Review third-party scripts, packages, and SVG assets before adoption.

## Credential exposure response

If a credential is committed or exposed, removing it in a later commit is not enough. Revoke or rotate it, review provider usage and access logs, restrict the replacement credential, and assess whether user data was accessed.

## Scope boundaries

The application does not book accommodation, confirm availability, or process payments. The absence of those capabilities is not a vulnerability. A future implementation of those capabilities requires a separate threat model, authorization design, and provider compliance review.

## Related guidance

See [docs/security.md](docs/security.md), [docs/authentication.md](docs/authentication.md), [docs/database.md](docs/database.md), and [docs/privacy.md](docs/privacy.md).
