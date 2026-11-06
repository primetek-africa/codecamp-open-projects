# SafeTrip Architecture

## Overview

SafeTrip is a mobile first safety app. The architecture must assume flaky networks and prioritize that alerts eventually reach contacts, even if the phone disconnects mid trip.

## Suggested stack

- Mobile. React Native with Expo. Background location via expo-location and Task Manager.
- API. Node with Fastify or NestJS.
- Database. PostgreSQL or MongoDB.
- SMS and WhatsApp. Twilio or Africa's Talking.
- Storage. S3 for audio uploads with client side encryption.
- Push. Expo push notifications.

## High level components

1. Mobile app. Trip, contacts, panic, history.
2. Trusted contact web page. Opens from a signed link, no account needed.
3. API service.
4. Alert dispatch service. Sends SMS, WhatsApp and push.
5. Escalation worker. Watches missed check ins and triggers next steps.

## Data model

- user. id, phone, name, locale.
- contact. id, user_id, contact_name, contact_phone, consent_at.
- trip. id, user_id, origin_point, dest_point, started_at, expected_arrival, status.
- checkin. id, trip_id, at_point, checked_at, missed.
- panic. id, trip_id or user_id, at_point, triggered_at, resolution.
- share_link. id, trip_id, token, expires_at.

## API surface

- POST /auth/otp
- POST /contacts
- POST /trips, POST /trips/:id/checkin, POST /trips/:id/complete
- POST /panic
- GET /share/:token

## Resilience

Panic alerts queue locally if offline and retry with exponential backoff. The phone also tries native SMS as a fallback when the network is slow but SMS over GSM is available. The share link page polls the API every 20 seconds while a trip is active.

## Privacy

Location stored encrypted at rest. Trip history only shown to the user. Contacts authenticate via signed tokens with expiry. Audio recordings stay on device unless the user explicitly uploads.

## Deployment

API on Render or Fly.io. Managed PostgreSQL. Expo EAS for mobile builds. Twilio or Africa's Talking for messaging.

## Scaling considerations

The system is mostly idle. Hot path is panic. Keep the panic path short and avoid expensive database writes in the critical section.
