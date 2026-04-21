# Pulse — Disclosure & Data Handling

This page describes how Pulse handles the health information it reads from Epic FHIR under a user's authenticated session.

## What Pulse collects from Epic

Read-only access to the user's own record, limited to the SMART-on-FHIR scopes the user authorizes:

`patient/Patient.read`, `patient/Observation.read`, `patient/MedicationRequest.read`, `patient/Condition.read`, `patient/AllergyIntolerance.read`, `patient/Appointment.read`, `patient/Immunization.read`, `patient/Procedure.read`

Pulse never writes to Epic. Pulse never queries records outside the authenticated user's own patient context.

## Where the data lives

- Stored in a Supabase / PostgreSQL instance operated by the Pulse owner
- Encrypted at rest (database-level) and in transit (TLS)
- Scoped per user via row-level security; users cannot see each other's data
- Self-hosted option available for users who require physical data control

## What Pulse does NOT do with the data

- Never used to train external AI / LLM models
- Never sold to third parties
- Never shared with advertisers, insurers, employers, or data brokers
- Never sent to any system outside the user's authorized Pulse instance, except as needed to render the app to the user's own browser

## Third-party services used to process data (all configured no-training)

- **Anthropic Claude API** — used for brief generation and chat. Calls are configured with Anthropic's no-training setting. Patient data is included in prompt context only for the duration of a single request and is not retained by Anthropic.

## Retention

- Health records are retained while the user's account is active
- On disconnect of an integration, tokens are revoked immediately; already-synced records remain unless the user deletes them
- Account deletion purges all associated health records within 30 days

## User rights

- **Export** — full export of all collected records is available on request, in JSON and human-readable formats
- **Delete** — users can delete individual records or their entire account at any time from Settings
- **Disconnect** — revoking any integration (Epic, Oura, Apple Health) stops future sync immediately

## Security

- OAuth 2.0 with PKCE (SMART-on-FHIR v1) for Epic authentication
- Tokens stored encrypted at the row level
- No client secrets used (public client, confidential-client flow not currently implemented)
- Audit log of every FHIR read tied to user ID and session

## HIPAA

Pulse operates under HIPAA-aligned architecture (encryption at rest + in transit, immutable audit log, per-user data isolation, no training use, incident response plan). A BAA is available on request for health systems that require one as a condition of integration.

## Contact

For security concerns, data-handling questions, or to request export / deletion / a BAA, contact the Pulse owner: alextsai at gmail dot com
