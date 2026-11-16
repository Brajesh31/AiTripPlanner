# Support and Project Help

## Before asking for help

Start with the [documentation index](docs/README.md). It explains setup, architecture, environment configuration, AI output, authentication, Firestore, deployment, security, accessibility, testing, and troubleshooting.

## Local setup

```bash
git clone https://github.com/Brajesh31/AiTripPlanner.git
cd AiTripPlanner
npm ci
npm run dev
```

Configure a local `.env` using placeholder values and a non-production provider project. See [Installation](docs/installation.md) and [Environment Configuration](docs/environment.md).

## Information to include

For a useful support request, provide:
- route and action being performed;
- browser and version;
- operating system;
- Node.js and npm versions if setup/build is involved;
- the commit or branch;
- exact reproduction steps;
- expected behavior and actual behavior;
- sanitized Console or Network error;
- relevant script output;
- whether the issue is reproducible with a synthetic test account.

Never include OAuth access tokens, API keys, service-account credentials, real user email addresses, private Firestore data, or personal itineraries.

## Where to report

- Use the GitHub issue templates for bugs, feature requests, and documentation corrections.
- Read [CONTRIBUTING.md](CONTRIBUTING.md) before submitting changes.
- Use [SECURITY.md](SECURITY.md) for suspected vulnerabilities. Do not disclose security problems publicly.

## Known project boundaries

The app creates AI-generated suggestions. It does not book hotels, guarantee availability or prices, or verify generated facts. Google Places autocomplete is imported but the current destination field is a standard text input. The app has no first-party backend API.

The current browser-side Gemini key exposure, localStorage-based profile checks, and missing committed Firestore rules are known production risks, not issues that should be hidden in support reports.

## Maintainer

**Brajesh Kumar**  
GitHub: [@Brajesh31](https://github.com/Brajesh31)

Support is maintained on a best-effort basis. No guaranteed response time or commercial support agreement is provided.
