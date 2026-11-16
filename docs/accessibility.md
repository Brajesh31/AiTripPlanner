# Accessibility Guide

## Objective
The interface should be usable by people navigating with keyboards, screen readers, zoom, and different viewport sizes. This guide defines review practices; it does not claim that the application passed a formal accessibility audit or conforms to a particular standard.

## Areas to review
Review the global header, navigation actions, Google sign-in dialog, trip form, budget/traveler selection cards, Places autocomplete suggestions, loading and error notifications, saved trip cards, and hotel/place details.

## Semantic structure
Use a clear primary heading per screen. Associate labels with each form field. Group related choices semantically. Use buttons for actions and links for navigation. Heading order should reflect content hierarchy, not desired font size.

## Keyboard operation
- Tab and Shift+Tab must follow a logical order.
- Enter and Space must activate appropriate controls.
- Dialogs must manage focus when opened and restore it when closed.
- Popovers must be operable and dismissible with Escape where appropriate.
- Autocomplete suggestions must support keyboard selection and communicate the active option.
- No keyboard trap may exist.

## Forms and validation
Every field needs an accessible label, not only placeholder text. Required status and validation errors must be programmatically associated with inputs. Error messages should explain how to correct a value. Do not communicate errors only through color or a short-lived toast.

The trip form should expose clear constraints for destination and duration. Prevent invalid numeric values and help users identify the field needing correction.

## Dialogs, popovers, and notifications
Radix components provide useful behavior, but composition still matters. Verify dialog titles/descriptions, close-button labels, focus behavior, popover trigger semantics, and toast announcements. A toast should not be the only way to communicate a critical error.

## Dynamic AI and data states
Announce meaningful loading and completion states. Ensure focus does not jump unexpectedly when generated content appears. Use headings for day sections and descriptive names for hotels and places. Handle missing values without rendering blank headings.

## Images
Informative images need concise alternative text. Decorative images should use empty alt text. Avoid generic text such as "image". Validate externally generated image URLs and provide useful fallbacks.

## Contrast and responsive design
Check text, buttons, input borders, focus indicators, and disabled states against actual backgrounds. Test narrow phone widths and 200% zoom. Avoid fixed heights that clip content and ensure long destination names wrap.

## Motion
Respect `prefers-reduced-motion`. Do not rely on animation alone to communicate status or progress.

## Manual review checklist
- [ ] All fields have associated labels.
- [ ] Required and invalid states are programmatically conveyed.
- [ ] Controls are keyboard accessible.
- [ ] Dialog focus is managed and restored.
- [ ] Popovers and autocomplete are keyboard usable.
- [ ] Toasts do not replace persistent critical errors.
- [ ] Headings and landmarks communicate structure.
- [ ] Images have suitable alternative text.
- [ ] Contrast is checked for text, borders, and focus.
- [ ] Content reflows at 200% zoom and narrow widths.
- [ ] Reduced-motion preferences are respected.
- [ ] Generated itinerary sections are structured sensibly.

For a significant review, record route, browser, assistive technology, viewport, test date, issue severity, and remediation. Do not claim conformance based solely on automated scans.
