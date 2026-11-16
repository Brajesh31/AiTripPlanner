# Performance Guide

## Performance profile
The application is a Vite React SPA with provider and UI dependencies. Cost areas can include the React bundle, icons, Firebase SDK, Google integrations, and external Gemini requests. Performance must be measured on a production build, not inferred from source size.

## Initial load
Inspect generated `dist/` output and the browser Network panel. Identify JavaScript chunk sizes, CSS size, image sizes, fonts, external resources, render-blocking requests, and unused dependencies. Consider route-level lazy loading if the initial route downloads screens the visitor has not opened.

## Images
The `public/` directory contains branding and placeholder images. Optimize dimensions and compression for rendered layouts. Avoid large originals in small cards. Add explicit dimensions or aspect ratios to reduce layout shift. Validate model-generated image URLs and provide local fallbacks.

## Provider latency
Gemini generation, OAuth profile requests, Places autocomplete, and Firestore operations introduce network latency. Show loading states, avoid duplicate requests, handle timeouts, and keep unrelated UI responsive. Do not retry non-idempotent writes blindly.

## React rendering
Avoid repeated state updates while iterating Firestore documents. Use stable keys and derived state where practical. Keep components focused to avoid unrelated work.

## Bundle review
Use Vite build output to inspect chunks. If bundle analysis tooling is introduced, document it and avoid committing reports unless needed. Lazy-load route screens only where measurement shows a benefit.

## Caching and hosting
Confirm caching headers at deployment. When an update appears stale, inspect the deployed commit, response headers, CDN cache, and browser cache before editing source.

## Measurement
Record deployed URL, commit, date, browser, device class, network settings, tool, repeated results, largest resources, and layout shifts. Do not claim Core Web Vitals results without reproducible measurements.

## Checklist
- [ ] Production build completes.
- [ ] Large images are appropriately sized.
- [ ] Unnecessary packages and resources are not added.
- [ ] Initial route avoids unneeded feature code where splitting is useful.
- [ ] Provider requests have loading and timeout behavior.
- [ ] Duplicate generation or writes are prevented.
- [ ] Static assets have suitable cache policies.
- [ ] Performance claims are based on recorded measurements.
