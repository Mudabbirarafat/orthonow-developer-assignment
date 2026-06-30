# OrthoNow Developer Assignment

**Candidate:** Mohd Mudabbir Arafat  
**Role Applied:** Developer – Position 1 (Client Web + Martech)  
**Submission Date:** 30 June 2026

This repository contains my submission for the Namoza Developer Assignment
(Position 1, Client Web + Martech). It includes the completed GTM event schema,
a working landing page, and an end-to-end integration design for the OrthoNow
"Book a Consultation" campaign.

## Repository

**GitHub:**  
https://github.com/Mudabbirarafat/orthonow-developer-assignment
**Loom Walkthrough:**  
https://www.loom.com/share/f92fd58201a8427f8a177bbfa728557b

## Contents

```
/
├── index.html                          # Task 02 — working landing page (open directly in browser)
├── docs/
│   ├── task-01-gtm-schema.md           # Task 01 — event schema and dataLayer JSON
│   ├── task-02-landing-page-notes.md   # Task 02 — implementation notes and testing
│   └── task-03-integration-design.md   # Task 03 — integration architecture writeup
├── assets/
│   └── pagespeed-mobile-score.png      # PageSpeed Insights Mobile screenshot
└── README.md                           # this file
```

## How to review

- `index.html` is fully self-contained — no build step or server required.
  Open it in a browser, open DevTools console, submit the form, and verify
  that `window.dataLayer` logs the expected event.
- The `docs/` directory contains the deliverables for each task, including the
  GTM event schema, landing page notes, and integration architecture.
- The landing page is designed to meet the assignment requirements while
  remaining lightweight, maintainable, and mobile-friendly.

## Quick review checklist

- [x] Task 01: GTM event schema with booking funnel dataLayer JSON
- [x] Task 01: Google Ads conversion recommendation with justification
- [x] Task 02: Working landing page with dataLayer push on successful submit
- [x] Task 02: PageSpeed Mobile screenshot included
- [x] Task 03: HubSpot integration design with phone-based deduplication approach
- [x] Task 03: Failure handling, monitoring, and fallback strategy documented

## Technologies Used

- HTML5
- CSS3
- Vanilla JavaScript
- Google Tag Manager (GTM)
- Google Analytics 4 (GA4)
- HubSpot CRM
- Google Ads
- Karix WhatsApp Business API

---
**Author:** Mohd Mudabbir Arafat
