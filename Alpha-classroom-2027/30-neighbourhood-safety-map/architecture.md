# Neighbourhood Safety Map Architecture

## Shape
A public area-level feed and private moderation dashboard use an API and database. A reporting module, safety resource directory, and aggregation module are sufficient for the MVP.

## Main modules and records
- Reports: category, coarse area, time, expiry, optional non-identifying note.
- Moderation status, reviewer, and removal reason.
- Safety resources with source and contact details.
- Aggregated counts with no reporter or subject identifiers.

## Main flow
A resident submits a report. Moderators remove identifying or unverified allegations and approve an area-level summary. Expired reports are removed from the active feed; the dashboard displays aggregated trends.

## Operations and safeguards
Do not build person tracking, face recognition, or public accusation features. Use strict moderation, short retention, coarse geolocation, access controls, and a clear process to correct or remove content.
