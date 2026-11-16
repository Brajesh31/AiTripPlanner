# Performance and Frontend Efficiency

## Scope
Performance depends on JavaScript bundle size, React rendering, image assets, third-party SDKs, network latency, Gemini generation, OAuth, and Firestore requests. Measure production builds under a documented browser, network, and device profile rather than relying on a development server.

## Current cost centers
Dependencies include React, Firebase, Gemini SDK, Axios, Google OAuth, Places packages, multiple icon packages, Radix UI components, Tailwind utilities, and toast libraries. Provider SDKs are imported into browser code. The initial Gemini service contains a lengthy embedded Las Vegas conversation which increases source and potentially bundle cost and influences prompt context.

## Build output
```bash
npm ci
npm run build
```
Vite writes production output to `dist/`. Inspect JavaScript and media sizes. Do not publish `node_modules/` as part of the website.

## Route-level splitting
Routes are statically imported in `src/main.jsx`. A future optimization could use React.lazy and Suspense, but measure first and provide loading states. Verify direct route navigation after splitting.

## Images
Trip cards currently use local placeholder imagery. Check dimensions, compression, and displayed size. Use responsive variants where useful and stable dimensions/aspect ratios to reduce layout shift. Do not assume model-supplied image URLs are valid.

## Provider latency
Gemini, OAuth, and Firestore calls depend on external services. Provide loading and error states, avoid duplicate requests, protect against stale responses, and use bounded timeouts/retries. Do not retry expensive generation indefinitely.

## React rendering
Avoid repeated state updates inside loops when one accumulated result can be set once. Use stable keys and review effect dependencies instead of disabling hook rules broadly. Add memoization only when measurement shows value.

## Dependency hygiene
The repository currently tracks a very large `node_modules/` tree. Generated dependencies do not belong in source control. Preserve `package-lock.json`, ignore `node_modules/`, and install with `npm ci`. Audit duplicates and unused packages before removing dependencies.

## Measurement
Record test URL/commit, browser, device class, network/CPU conditions, tool version, date, asset sizes, route measured, and repeated results. Do not claim a performance score without evidence.

## Checklist
- [ ] Production build succeeds.
- [ ] Largest assets are inspected.
- [ ] Embedded prompt history is reviewed.
- [ ] Images are appropriately sized.
- [ ] Third-party SDKs are justified.
- [ ] Loading and failure states are usable.
- [ ] Generated dependencies are excluded from deployment.
- [ ] Claims are backed by repeatable measurements.
