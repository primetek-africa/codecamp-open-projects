# EventPass Architecture

## Overview

EventPass is a checkout plus ticket store plus scanner. The architectural concerns are secure ticket generation and reliable offline scanning at the gate.

## Suggested stack

- Front end. Next.js.
- Scanner app. Expo React Native with camera module.
- API. Node or Laravel.
- Database. MySQL or PostgreSQL.
- Payments. Paystack, Flutterwave or M-Pesa Daraja.
- Storage. Cloudinary for event posters.
- QR. qrcode library.

## High level components

1. Public site. Event list, event detail, checkout.
2. Promoter dashboard. Event setup, sales, refunds.
3. Scanner app. Download event, scan at gate.
4. API service.
5. Payment and webhook service.

## Data model

- promoter. id, name, phone, settlement_account.
- event. id, promoter_id, name, starts_at, venue, poster_url, status.
- tier. id, event_id, name, price, capacity, sold.
- order. id, event_id, customer_phone, customer_email, total, status.
- ticket. id, order_id, tier_id, serial, code, status (unused, used, void).
- scan_log. id, ticket_id, scanner_id, scanned_at, device_id.

## API surface

- GET /events
- POST /orders, POST /payments/initiate
- POST /webhooks/payment
- POST /scan with a signed ticket code
- GET /promoters/:id/sales

## Ticket generation

On successful payment, generate one ticket per seat with a random code and a HMAC signature. QR encodes the code. The scanner app downloads the ticket set per event ahead of the gates opening and can scan offline, then syncs scan logs when the network returns. Server side double scan protection runs when sync lands.

## Deployment

Standard managed stack. Payment provider live or sandbox. Scanner app signed and installed on usher phones.

## Scaling considerations

Checkout bursts around the drop time. Rate limit per IP. Use a queue for order creation if the event is very large. Precompute remaining capacity per tier in cache.
