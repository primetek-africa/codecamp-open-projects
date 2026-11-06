# FarmLink Architecture

## Overview

FarmLink is a two sided marketplace with a messaging layer, a simple listing engine and a payment hand off. It is not a logistics company. Delivery is delegated.

## Suggested stack

- Front end. Nuxt or Next.js. Farmer app is a PWA friendly to low end Android.
- API. Laravel or Node.
- Database. MySQL.
- Search. MySQL full text to start.
- Messaging. Pusher or Socket.IO.
- Payments. Paystack, Flutterwave or M-Pesa Daraja.
- Storage. Cloudinary for photos.
- SMS. Africa's Talking or Termii.

## High level components

1. Farmer PWA. List, respond, confirm.
2. Buyer web app. Browse, request, order.
3. Admin panel. Moderation, disputes.
4. API service.
5. Messaging service.

## Data model

- farmer. id, name, phone, farm_name, county, lat, lng, verified_at.
- buyer. id, name, phone, business_type, city.
- listing. id, farmer_id, crop, grade, qty_kg, unit_price, available_from, available_until, photos_json, status.
- request. id, buyer_id, crop, qty_kg, budget, deadline, status.
- offer. id, listing_id, request_id, price, qty_kg, status.
- order. id, offer_id, pickup_or_deliver, pickup_at, price_total, commission, status.
- message. id, thread_id, author_id, body, created_at.

## API surface

- POST /auth/otp
- POST /listings
- POST /requests
- POST /offers, POST /offers/:id/accept
- POST /orders/:id/confirm
- POST /messages

## Deployment

Standard managed stack. Managed MySQL. Cloudinary for images. Payment provider sandbox for the MVP.

## Scaling considerations

Expect low volume at first but seasonal spikes at harvest. Add indexes on listing(crop, available_from, status) and request(crop, deadline). Cache search results for 60 seconds.
