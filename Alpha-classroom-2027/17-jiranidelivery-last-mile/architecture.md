# JiraniDelivery Architecture

## Overview

JiraniDelivery is a dispatch platform similar in shape to ride hailing but simpler because trips are short and few. The focus is reliable dispatch and transparent tracking.

## Suggested stack

- Mobile for courier. React Native with Expo.
- Merchant dashboard and customer tracking page. Next.js.
- API. Node with Nest or Fastify. Socket.IO for live tracking.
- Database. PostgreSQL with PostGIS.
- Cache. Redis for live courier locations.
- Maps. Mapbox.
- Payments. Paystack or M-Pesa Daraja.

## High level components

1. Merchant dashboard. Orders, settlement, support.
2. Courier app. Available toggle, order queue, navigate, deliver.
3. Customer tracking page. Live map and ETA.
4. API service.
5. Dispatch service.
6. Settlement job.

## Data model

- merchant. id, business_name, phone, address, settlement_method.
- courier. id, phone, vehicle_type, home_city, status, verified_at.
- order. id, merchant_id, customer_phone, dropoff (point), items_text, pickup_time, fee.
- delivery. id, order_id, courier_id, picked_at, delivered_at, proof_url, status.
- location_ping. id, courier_id, point, pinged_at (hot in Redis, cold in postgres daily).
- payout. id, courier_id, amount, period, status.

## API surface

- POST /orders
- POST /dispatch/attempt/:order_id
- WS /courier/stream
- POST /deliveries/:id/proof
- GET /track/:token

## Dispatch

On order creation, the dispatch service finds nearby online couriers filtered by vehicle type, sorts by distance to pickup, and sends an offer to the top candidate with a 20 second timeout. If declined or expired, move to the next. Record every attempt for audit and analytics.

## Deployment

Standard managed stack. Mobile via Expo EAS. Realtime via Socket.IO. Payments via provider sandbox or live.

## Scaling considerations

Expect bursts around lunch and evening. Precompute courier buckets per city. Keep location ping writes in Redis and roll up to PostgreSQL every 5 minutes.
