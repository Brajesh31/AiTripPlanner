# 📘 ♿ Accessibility Guide

## 📌 📌 Objective
AI Trip Planner includes navigation, a trip form, custom choice cards, a sign-in dialog, a profile popover, asynchronous generation, saved trip cards, and nested itinerary content. Accessibility must cover the whole interaction flow. This guide is a development standard, not an audit or certification.

## 📌 📌 Semantic structure
Use meaningful landmarks and heading hierarchy. Each route should have a clear page title and main content. Every form field needs a visible label. Use anchors for navigation and buttons for actions. Avoid clickable generic containers unless keyboard and accessible state semantics are fully implemented.

## 📌 📌 Trip form
- Associate a label with destination.
- Label the day-count input and constrain it to an appropriate positive integer.
- Expose budget choices as a semantic selectable group.
- Expose traveler choices with clear names and selected states.
- Announce validation errors and associate them with fields.
- Communicate generation/loading state.
- Prevent accidental repeated submission.
- Announce success and failure without relying only on color.

The current form uses custom clickable option cards. Review their roles, focusability, keyboard support, and accessible names before claiming full keyboard accessibility.

## 📌 📌 Authentication dialog
Radix Dialog can provide focus management and dialog semantics when used correctly. Give the dialog a meaningful title and description. Confirm that opening moves focus into the dialog, closing returns focus to its trigger, Escape behavior works, and the sign-in button has a clear name. The current header has an empty dialog title that should be addressed in a future UI correction.

## 📌 📌 Header and profile popover
The profile trigger needs an accessible name. Provide meaningful alt text for user imagery or an empty alt when decorative. Logout should be a real button, not a clickable heading. Ensure navigation remains clear when signed out.

## 📌 📌 Async content
Trip generation and Firestore fetches take time. Expose busy states with suitable status semantics. Provide useful loading, empty, success, and failure messages. Unlabeled visual skeletons should not be the only information exposed to assistive technology.

## 📌 📌 Generated itinerary
Use headings for days and semantic lists for places where appropriate. Label price, travel time, and other fields consistently. Do not rely on emoji alone to communicate meaning. External links should make their destination clear.

## 📌 📌 Keyboard and focus
Test Tab/Shift+Tab order, Enter/Space activation, Escape for dialogs/popovers, and visible focus. Avoid focus traps outside modal dialogs. Do not remove outlines or ring styles without an equivalent replacement.

## 📌 📌 Color, contrast, and motion
Check actual text/control contrast. Do not use color alone for errors or selected budget/traveler choices. Respect reduced-motion preferences and avoid unnecessary animation during generation or navigation.

## 📌 📌 Responsive and zoom
Test phone, tablet, desktop, and 200% zoom. Long destination names and generated itinerary text must wrap without overlapping controls or requiring unintended horizontal scrolling.

## 📌 📌 Screen-reader review
For significant changes, inspect the app with a screen reader. Confirm route headings, labels, choice state, dialog title, profile control, loading announcements, and trip-detail hierarchy are understandable.

## 📌 📌 Checklist
- [ ] Every form input has an associated label.
- [ ] Custom choices are keyboard-operable and expose selected state.
- [ ] Validation errors are associated with controls.
- [ ] Dialogs have meaningful accessible names and descriptions.
- [ ] Popover triggers have accessible names.
- [ ] Loading, empty, and error states are announced.
- [ ] Focus is visible and ordered logically.
- [ ] Text and controls have sufficient contrast.
- [ ] Layout works at narrow widths and 200% zoom.
- [ ] Reduced motion is respected.
- [ ] Generated itinerary uses semantic structure.
