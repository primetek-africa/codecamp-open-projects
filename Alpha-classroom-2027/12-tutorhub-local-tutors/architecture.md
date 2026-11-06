# TutorHub Architecture

## Overview

TutorHub is a two sided marketplace with a verification workflow, chat and a booking fee. The main architectural concern is search and trust.

## Suggested stack

- Front end. Nuxt or Next.js.
- API. Laravel or Node with Nest.
- Database. MySQL.
- Search. MySQL full text for the MVP. Meilisearch or Typesense later.
- Chat. Pusher, Laravel Reverb or Socket.IO.
- Payments. Paystack or Flutterwave.
- Storage. Cloudinary.

## High level components

1. Web app with parent, tutor and admin views.
2. API service.
3. Chat service.
4. Payment service.
5. Admin panel for verification.

## Data model

- user. id, phone, email, role, locale.
- tutor_profile. id, user_id, bio, subjects_json, grades_json, languages_json, hourly_rate, city, lat, lng, verified_at.
- reference. id, tutor_id, name, phone, relationship, verified_at.
- availability. id, tutor_id, day_of_week, start_time, end_time.
- lesson_request. id, parent_id, tutor_id, subject, grade, preferred_time, status.
- booking. id, request_id, price, service_fee, payment_id, status.
- payment. id, booking_id, provider, provider_ref, amount, status.
- review. id, booking_id, stars, body, created_at.
- message. id, thread_id, author_id, body, created_at.

## API surface

- POST /auth/otp
- POST /tutors/profile
- GET /tutors?subject=math&lat=...&lng=...
- POST /requests, PATCH /requests/:id/accept
- POST /bookings/:id/pay, POST /webhooks/payment
- POST /reviews

## Search

Index tutors on subject, grade, language, location and verified status. Rank by rating, completed lessons and distance. Hide tutors without verification from the default search and only show them on a see all pending view.

## Deployment

Standard managed stack. Vercel for front end, Render for API and workers, managed MySQL. Pusher cheap tier for chat. Payment provider test keys for the MVP.

## Scaling considerations

Searches and messages are the hot path. Cache tutor cards for 60 seconds. Paginate message history. For real growth, split chat onto its own service and index tutors into Meilisearch.
