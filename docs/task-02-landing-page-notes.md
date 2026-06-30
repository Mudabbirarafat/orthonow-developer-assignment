# Task 02 — Landing Page

The working implementation is located at the repository root as `index.html`.
It is a single self-contained HTML file that can be opened directly in any
modern browser without a server or build step.

## How to test

1. Open `index.html` in a browser (mobile viewport recommended via DevTools).
2. Open the browser console.
3. Fill in the Name and Phone fields, then submit the form.
4. Confirm that `window.dataLayer` logs a `consultation_form_submitted` event
   only after a successful submission.
5. Try submitting with an invalid phone number (e.g. 9 digits) and verify that
   validation prevents submission and no `dataLayer` event is pushed.
6. Resize the browser to a mobile width and verify that the sticky CTA appears
   while the booking card switches to the mobile layout.
7. Run the page through Google PageSpeed Insights (Mobile) and compare the
   result with the included screenshot.

## Design decisions

- **No web fonts loaded** — a system font stack is used to reduce external
  requests and help maintain a high PageSpeed Mobile score.
- **Headline is pain-point focused** ("Still pushing through knee or back pain
  to get to your desk?") to address the target audience directly rather than
  using a generic healthcare headline.
- **Minimal lead form** — two required fields (Name and Phone) with one optional
  Clinic Preference field to reduce friction while still collecting essential
  information.
- **Tap-to-call number** in the utility bar provides an alternative conversion
  path for users who prefer calling instead of submitting a form.
- **Today's Open Slots** section provides an availability indicator without
  relying on artificial urgency techniques such as countdown timers.

## Implementation Notes

### Performance

The landing page is designed to achieve a strong Google PageSpeed Mobile score
by minimizing render-blocking resources, avoiding external font libraries, and
keeping the page lightweight.

### GTM Integration

The `window.dataLayer.push()` event is triggered only after a successful form
submission and includes meaningful event parameters for GTM and GA4 tracking.

### Form Validation

The form validates the phone number before submission. Invalid input prevents
both the submission flow and the associated GTM event from firing.

### Code Organization

- HTML uses semantic structure where appropriate.
- CSS is organized into logical sections.
- JavaScript separates validation, UI updates, and analytics events for better
  readability and maintainability.