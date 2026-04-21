# Pulse — Product Documentation

Pulse is a personal AI health assistant. It combines wearable data (Oura, Apple Health), calendar, email, and clinical records into a single daily brief, surfacing trends, tensions, and proactive recommendations that would otherwise require a human assistant to spot.

## What the app reads from Epic (FHIR R4)

Pulse reads the following FHIR R4 resources under the authenticated user's patient context:

- **Patient** — demographics used to personalize the /health surface
- **Observation** (Labs) — lab results for trend analysis, reference-range flagging
- **Observation** (Vitals) — blood pressure, weight, heart rate trended against wearable signals
- **MedicationRequest** — active medications for brief context and interaction checking
- **Condition** — active diagnoses, surfaced only when relevant
- **AllergyIntolerance** — surfaced for procedure prep and meeting context
- **Appointment** — merged into Pulse's calendar view
- **Immunization** — displayed on the user's /health timeline
- **Procedure** — displayed on the /health timeline, used for recovery framing

Read-only. Pulse never writes back to Epic. Pulse never performs bulk FHIR or patient matching outside the user's own authorized record.

## Primary user value

- **Daily brief** — health, calendar, tasks, and clinical signals synthesized into a 3-5 sentence narrative
- **Trend detection** — sleep, HRV, labs, vitals pulled together into simple, readable charts
- **Pattern detection** — surfaces when recurring health signals correlate with life events (e.g., declining HRV in weeks with poor sleep)
- **Proactive recommendations** — flags when a high-stakes day and a declining recovery signal overlap

## User control

- Users can disconnect Epic at any time from Settings → Integrations
- Full export of all health data available on request
- Delete account purges all stored data within 30 days
- Data stays on the user's Pulse instance and is never used to train external models

## Technology

- Next.js + React (client)
- Node.js + tRPC (server)
- Supabase / PostgreSQL (storage, encrypted at rest)
- SMART-on-FHIR OAuth 2.0 with PKCE (public client)

## Contact

alextsai at gmail dot com
