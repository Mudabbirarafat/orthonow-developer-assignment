# Task 01 — GTM Event Schema

This document outlines the GTM event tracking strategy for OrthoNow's website,
including the event schema, booking funnel tracking, dataLayer implementation,
and the recommended Google Ads conversion event.

## Full Event Table

| Event Name | Trigger Type | Key Parameters | GA4 Destination |
|---|---|---|---|
| `call_now_click` | Click trigger (Click ID/Class = `.call-now-btn`) | `page_location`, `clinic_name` (if on clinic page), `button_position` (header/footer/sticky) | Engagement report + "High Intent" audience |
| `whatsapp_widget_click` | Click trigger on floating widget element | `page_location`, `referrer_page_type`, `time_on_page` | Engagement report + remarketing audience |
| `patient_guide_form_submit` | Form Submit trigger (or custom dataLayer push if AJAX) | `lead_name` (excluded from GA4 if PII-sensitive), `phone_provided` (boolean, not the number), `guide_topic` | Conversions report; feeds "Guide Downloaders" audience |
| `patient_guide_download` | Custom Event, fired after gated form success | `file_name`, `guide_topic`, `download_method` | Engagement / content performance report |
| `clinic_page_view` | History Change / Page View trigger filtered by URL pattern `/clinics/*` | `clinic_name`, `clinic_city`, `page_path` | Standard Pages report; "Clinic Interest by City" audience |
| `blog_scroll_depth` | Scroll Depth trigger (25/50/75/90%) | `scroll_percentage`, `article_title`, `article_category` | Engagement report; "Engaged Readers" remarketing audience |
| `booking_step_complete` | Custom Event (dataLayer push from front-end developer) | `step_number`, `step_name`, plus step-specific fields | GA4 Funnel Exploration (primary use) |
| `booking_confirmed` | Custom Event, fired on backend success response | `clinic_location`, `specialty`, `appointment_date`, `booking_id` | Conversions report — **Google Ads import candidate** |
| `booking_failed` | Custom Event, fired on backend error | `error_type`, `step_number`, `clinic_location` | Ops/debug dashboard (not a GA4 audience source) |

Additional events such as scroll depth and booking failure have been included to
provide deeper visibility into user engagement, content performance, and booking
funnel reliability.

## Booking Funnel — dataLayer JSON

For a multi-step JavaScript booking form, GTM cannot automatically detect step
transitions. The front-end application should explicitly trigger
`window.dataLayer.push()` whenever a user completes a booking step. GTM then
listens for these events using Custom Event triggers and forwards them to GA4.

```json
// Step 1 - location + specialty selected
{
  "event": "booking_step_complete",
  "step_number": 1,
  "step_name": "location_specialty_selected",
  "clinic_location": "Indiranagar - Bengaluru",
  "specialty": "Knee & Joint Care"
}
```

```json
// Step 2 - contact details entered
{
  "event": "booking_step_complete",
  "step_number": 2,
  "step_name": "contact_details_entered",
  "clinic_location": "Indiranagar - Bengaluru",
  "preferred_date": "2026-07-05"
}
```

```json
// Step 3 - booking confirmed
{
  "event": "booking_confirmed",
  "step_number": 3,
  "step_name": "booking_confirmed",
  "clinic_location": "Indiranagar - Bengaluru",
  "specialty": "Knee & Joint Care",
  "booking_id": "ORTH-2026-08841"
}
```

**GTM setup:** Configure Custom Event triggers for each event name and map them
to GA4 Event tags with the corresponding parameters.

**GA4 Funnel Exploration:** Create an open funnel using:

- `booking_step_complete` (Step 1)
- `booking_step_complete` (Step 2)
- `booking_confirmed` (Step 3)

This makes it easy to identify where users abandon the booking process and helps
prioritize UX improvements based on step-level conversion rates.

## Google Ads Conversion Import

**Recommended conversion:** `booking_confirmed`

**Reason:** This event represents a confirmed appointment, making it the most
valuable business outcome for campaign optimization. Importing softer intent
signals such as form submissions or intermediate booking steps could cause
Google Ads to optimize toward users who never complete a booking, reducing
overall campaign efficiency.