# DocDirect Architecture

## Overview

DocDirect combines a booking engine, a video consultation runtime and a payment settlement flow. Each is simple on its own. The architectural challenge is making the video session robust on low bandwidth networks.

## Suggested stack

- Front end. React or Next.js. Patient PWA with large tap targets. Clinician dashboard on a separate route with denser information.
- API. Django with DRF or Node with NestJS.
- Database. PostgreSQL.
- Video. Daily.co, Agora, 100ms or a self hosted Jitsi. Daily.co is the easiest to integrate for an MVP.
- Payments. Paystack, Flutterwave or M-Pesa Daraja.
- Queue. Celery or BullMQ.
- Storage. S3 compatible for ID uploads.

## High level components

1. Patient app. Browse, book, consult, pay, follow up.
2. Clinician app. Schedule, consultation, notes, payouts.
3. Admin panel. Verification queue, disputes.
4. API service.
5. Payment service that creates a payment intent, verifies via webhook and books the slot on success.
6. Video gateway that creates and expires rooms.
7. Payout service that batches clinician payouts weekly.

## Data model

- user. id, phone, email, role.
- clinician_profile. id, user_id, council_no, specialties, bio, verified_at.
- availability_slot. id, clinician_id, starts_at, ends_at, status.
- booking. id, patient_id, slot_id, specialty, price, payment_id, status.
- payment. id, booking_id, provider, provider_ref, amount, status.
- consultation. id, booking_id, video_room_id, started_at, ended_at, notes.
- payout. id, clinician_id, period_from, period_to, amount, status.

## API surface

- POST /auth/otp
- POST /clinicians/apply
- GET /clinicians?specialty=derm
- POST /bookings, POST /payments/initiate, POST /webhooks/payment
- GET /consultations/:id/token
- POST /consultations/:id/notes

## Video

The video gateway service exposes POST /rooms which calls Daily.co or Agora to create a room with a short lived token. Tokens are issued per participant and expire 30 minutes after the booking end time. For low bandwidth clients, prefer audio only and 180p video. Include a client side network quality meter and fall back automatically.

## Payments

Patient initiates payment, the payment service creates an intent with the provider and returns a redirect or inline code. The webhook on success flips the booking to paid and creates the slot lock. If the webhook does not arrive within 5 minutes, poll the provider for the status.

## Deployment

Patient and clinician apps on Vercel or Netlify. API and workers on Render, Fly.io or AWS. Managed PostgreSQL with point in time recovery. S3 for uploads. Video via a provider to avoid running TURN servers in the MVP.

## Scaling considerations

Video is the expensive leg at scale. Measure the completion rate per network quality and only then optimize. Booking is write heavy on slot lock. Use a transaction with SELECT FOR UPDATE on slot rows to prevent double booking.
