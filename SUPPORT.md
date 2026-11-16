# 🆘 Support and Project Help

## Start with documentation

Read the [documentation index](docs/README.md). It covers installation, routes, architecture, Google authentication, Gemini generation, data structure, Firestore, environment setup, deployment, security, testing, accessibility, and troubleshooting.

## Local setup

```bash
git clone https://github.com/Brajesh31/AiTripPlanner.git
cd AiTripPlanner
npm ci
cp .env.example .env
npm run dev
```

Configure the required development provider values before testing OAuth, Gemini, Places, and Firestore. Do not commit the populated `.env` file.

## What to include in a support request

Include:
- route and exact action performed;
- browser and version;
- operating system;
- Node and npm versions;
- commit or branch;
- numbered reproduction steps;
- expected and actual result;
- relevant sanitized Console/Network errors;
- whether the issue reproduces after a clean `npm ci`.

Remove tokens, API keys, email addresses where unnecessary, and private itinerary details before sharing evidence.

## Where to ask

- Use issue templates for bugs, feature requests, or documentation corrections.
- Read [CONTRIBUTING.md](CONTRIBUTING.md) before submitting code.
- Use [SECURITY.md](SECURITY.md) for suspected vulnerabilities. Do not report active credentials or exploits in public issues.

## Known product boundaries

AI Trip Planner produces itinerary suggestions. It does not reserve hotels, confirm live availability, sell tickets, or process payments. AI-generated prices, ratings, locations, images, and travel details require independent verification.

## Maintainer

**Brajesh Kumar**  
GitHub: [@Brajesh31](https://github.com/Brajesh31)

Support is provided on a best-effort basis. No guaranteed response time or commercial support commitment is offered.
