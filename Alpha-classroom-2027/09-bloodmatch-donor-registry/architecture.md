# BloodMatch Architecture

## Overview

BloodMatch is a notification fan out service with a small verification workflow. The speed of a request fan out is the key performance measure.

## Suggested stack

- Donor mobile app. React Native with Expo. Push notifications via Expo.
- Hospital portal. Nuxt or Next.js.
- API. Node with Fastify or Nest.
- Database. MongoDB or PostgreSQL. Either works. PostgreSQL with PostGIS is a good default.
- SMS. Africa's Talking or Twilio.
- WhatsApp. Twilio WhatsApp Business.
- Queue. BullMQ.

## High level components

1. Donor app. Sign up, status, respond.
2. Hospital portal. Verify, create requests, confirm donations.
3. Admin panel. Hospital verification, abuse review.
4. API service.
5. Fan out worker that resolves eligible donors and queues notifications.
6. Response ingest service that updates the request and marks donors as pending.

## Data model

- donor. id, phone, blood_type, city, lat, lng, age, cooldown_until, consent_at, status.
- hospital. id, name, city, verified_at.
- staff. id, hospital_id, email, role.
- request. id, hospital_id, blood_type, qty_units, radius_km, urgency, status, created_at.
- notification. id, request_id, donor_id, channel, status, sent_at.
- response. id, request_id, donor_id, status (available, not_available, snooze).
- donation_record. id, request_id, donor_id, confirmed_at.

## API surface

- POST /auth/donor/otp
- POST /donors, PATCH /donors/me
- POST /hospitals/apply, POST /hospitals/:id/verify
- POST /requests, GET /requests/:id/responses
- POST /responses, POST /donations

## Fan out

The request handler writes the request row and enqueues a fan out job. The worker computes eligible donors with a geo query plus blood type and cooldown filter, then enqueues a notification job per donor. Keep the per request cap at a sensible number like 500 for the MVP to control cost.

## Privacy

Store exact donor location only if the donor opts in. Default to city level location. Hospitals see an anonymized responder list until the donor arrives in person.

## Deployment

API and workers on Render or Fly.io. Managed PostgreSQL or MongoDB Atlas. Push via Expo. Messaging via provider API. Static sites on Vercel.

## Scaling considerations

Fan out is the hot path. Ensure the geo filter is indexed. Rate limit per hospital to prevent abuse. For very large campaigns, partition notifications by city and send with a staggered schedule.
