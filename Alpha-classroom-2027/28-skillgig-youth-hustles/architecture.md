# SkillGig Architecture

## Overview

SkillGig is a marketplace with messaging, booking and a small payment flow. Architecture is similar to TutorHub and FarmLink.

## Suggested stack

- Front end. Nuxt or Next.js.
- API. Laravel or Node.
- Database. MySQL.
- Messaging. Pusher or Socket.IO.
- Payments. Paystack or Flutterwave.
- Storage. Cloudinary for portfolio photos.

## High level components

1. Worker PWA. Profile, apply, messages.
2. Employer web app. Post, shortlist, pay.
3. Admin panel. Verification, abuse review.
4. API service.
5. Messaging service.

## Data model

- worker. id, user_id, bio, skills_json, city, lat, lng, verified_at.
- portfolio_item. id, worker_id, title, photo_url, link.
- reference. id, worker_id, name, phone, relationship, verified_at.
- gig. id, employer_id, title, category, city, budget, deadline, status.
- application. id, gig_id, worker_id, proposed_rate, message, status.
- booking. id, application_id, price, service_fee, payment_id, status.
- review. id, booking_id, by, stars, body.
- message. id, thread_id, author_id, body.

## API surface

- POST /auth/otp
- POST /workers/profile
- POST /gigs
- POST /applications
- POST /bookings/:id/pay
- POST /messages

## Deployment

Standard managed stack. Payment provider sandbox for the MVP.

## Scaling considerations

Messaging is the hot path. Cache gig lists. Paginate messages. Index gig(city, category, status) and application(gig_id, worker_id).
