# Task 03 — Integration Design

This document describes the proposed end-to-end integration architecture for the
OrthoNow consultation booking flow, covering HubSpot CRM, Karix WhatsApp,
Google Ads, and GA4 while meeting the assignment requirements.

## End-to-end architecture: Landing Page → HubSpot → Karix WhatsApp → Google Ads → GA4

A direct backend API implementation, rather than a no-code integration tool, is
the preferred approach. The form submits to a lightweight serverless function
(e.g. Vercel Functions, Cloudflare Workers, or AWS Lambda) instead of embedding
a native HubSpot form or routing through Zapier/Make. This provides greater
control over retries, logging, sequencing, and reliability while helping meet
the required 2-minute SLA.

**Flow, in order:**

1. The landing page submits the form to a backend endpoint using `fetch()`,
   ensuring HubSpot credentials remain server-side.
   Input validation and rate limiting should also be implemented at the backend to protect the endpoint from malformed requests and abuse.
2. The backend searches HubSpot Contacts using the phone number before deciding
   whether to create or update a contact. Since HubSpot's default
   deduplication is email-based, a custom search
   (`POST /crm/v3/objects/contacts/search`) is performed on the phone property
   before calling the Contacts API.
3. After the CRM operation completes, the backend triggers two parallel
   processes: a Karix WhatsApp Business API request for the confirmation
   message and a server-side Google Ads Enhanced Conversion (or GTM
   Server-Side event) for `consultation_form_submitted`.
4. A GA4 Measurement Protocol request records the same conversion server-side,
   helping preserve analytics data even if the browser session ends before
   client-side tags execute.
5. Each stage records its status using a submission ID so failures can be
   traced and retried if necessary.

## Biggest failure point and fallback

The most significant failure point is the HubSpot contact write. If it fails or
times out, the WhatsApp confirmation and Google Ads conversion should continue
independently so the patient still receives confirmation and marketing
measurement is preserved. HubSpot writes are retried using exponential backoff
(up to three attempts). If all retries fail, the lead is stored in a fallback
database or operational queue and an alert is generated so the record can be
processed manually.

## WhatsApp SLA monitoring

Potential causes of exceeding the 2-minute SLA include Karix API latency, rate
limiting during high traffic, or WhatsApp template approval and delivery
issues. Monitoring should track the time between form submission and the
WhatsApp delivery webhook. If delivery exceeds 90 seconds, an alert is raised,
providing sufficient time to investigate before the 2-minute SLA is breached.

## Phone-based deduplication

If two submissions use the same phone number with different names, the backend
searches HubSpot using the phone number and updates the existing contact rather
than creating a duplicate. The system should also flag these cases for manual
review, as they may indicate a shared family phone, a typing error, or a
booking made on behalf of another patient. Using phone as the deduplication key
ensures consistent contact management for healthcare lead generation.