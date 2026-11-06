# SafeSOS Architecture

## Overview

SafeSOS has a very similar architecture to SafeTrip but focuses on an always available panic path rather than a trip timeline. The key design concern is ensuring the panic button works even when the app is backgrounded and the network is poor.

## Suggested stack

- Mobile. React Native with Expo or native Android first. Background tasks with Expo Task Manager or Android foreground services.
- API. Node with Fastify.
- Database. MongoDB or PostgreSQL.
- SMS and WhatsApp. Twilio, Africa's Talking or Termii.
- Storage. S3 compatible for audio.
- Push. Expo push or Firebase Cloud Messaging.

## High level components

1. Mobile app. Setup, button, history, map.
2. API service.
3. Alert dispatch service.
4. Audio ingest service with scoped signed URLs.
5. Admin panel for support.

## Data model

- user. id, phone, name, locale, plan.
- contact. id, user_id, name, phone, consent_at.
- panic_event. id, user_id, lat, lng, triggered_at, canceled, resolution.
- alert_delivery. id, panic_id, contact_id, channel, status.
- audio_clip. id, panic_id, url_encrypted, duration_seconds.
- safe_zone. id, name, city, lat, lng, type, verified_at.

## API surface

- POST /auth/otp
- POST /contacts
- POST /panic
- PATCH /panic/:id/cancel
- POST /audio for upload
- GET /safe-zones?city=

## Panic path

The panic request is the hot path. On tap, the mobile app sends the request with location. If offline, queue locally with exponential backoff. The server fan outs via SMS, WhatsApp and push in parallel. Record every attempt. SMS works on 2G, so prioritize it on known slow networks.

## Privacy

Audio is encrypted at rest with a per user key. Audio can be deleted by the user at any time. Trusted contacts never see each other's numbers. Location history is retained only for events, not continuous tracking.

## Deployment

Mobile via Expo EAS or Google Play. API on Render or Fly.io. Managed database. Push via Expo or FCM. SMS via provider API.

## Scaling considerations

The system is idle most of the time. Make sure the panic queue has enough workers to handle a spike during a city wide incident. Rate limit per user to prevent spam.
