# ♿ Accessibility Quality Assurance

A manual checklist for forms, dialogs, navigation, and generated itinerary content. This is a test plan, not a claim of formal accessibility certification.

## Keyboard testing

- [ ] Every interactive control can be reached with Tab.
- [ ] Focus follows visible reading order.
- [ ] Enter and Space activate appropriate controls.
- [ ] Escape closes dismissible dialogs and popovers.
- [ ] Focus returns to the dialog trigger.
- [ ] Focus is not trapped behind an open modal.
- [ ] A visible focus indicator is present.

## Forms

- [ ] Every input has a programmatic label.
- [ ] Required fields are identified with text, not color alone.
- [ ] Validation explains how to correct a value.
- [ ] Loading state is exposed to assistive technology.
- [ ] Submit cannot be triggered repeatedly by accident.
- [ ] Numeric values have sensible bounds.
- [ ] Selected budget and traveler choices are announced.

## Generated content

- [ ] Day headings are semantic and ordered.
- [ ] Hotel and place cards have descriptive headings.
- [ ] Informative images have useful alternative text.
- [ ] Decorative images are hidden from assistive technology.
- [ ] Location links have understandable names.
- [ ] Price and time are not conveyed by color alone.
- [ ] Long descriptions remain readable at 200% zoom.

## Visual and responsive checks

- [ ] Text contrast targets WCAG AA for normal text.
- [ ] Controls have adequate contrast and clear states.
- [ ] Layout works at 320 CSS pixels without unintended horizontal scrolling.
- [ ] Increased text spacing does not clip content.
- [ ] Reduced-motion preferences are respected.
- [ ] Touch targets are sufficiently large and separated.

## Manual matrix

| Input method | Browser/tool | Scenarios |
|---|---|---|
| Keyboard only | Chrome / Firefox | Create trip, dialog, route navigation |
| Screen reader | NVDA / VoiceOver | Labels, headings, errors, itinerary |
| Zoom | Browser zoom | 200% and 400% |
| Mobile viewport | Chrome DevTools | Form, navigation, trip cards |
| High contrast | OS/browser setting | Focus, selection, errors |

## Reporting

Include route/component, browser and assistive technology, exact keyboard sequence, expected and actual behavior, sanitized evidence, and whether the issue blocks task completion.
