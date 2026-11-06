# LocalEats Architecture

## Overview

LocalEats is a food ordering platform with a delivery hand off. It shares shape with JiraniDelivery, but adds a restaurant kitchen workflow and an order lifecycle.

## Suggested stack

- Front end. Next.js.
- Restaurant tablet app. PWA on a cheap Android tablet.
- Courier app. Expo React Native, shared with JiraniDelivery if possible.
- API. Node with Nest or Laravel.
- Database. PostgreSQL.
- Payments. Paystack or M-Pesa Daraja.
- Realtime. Socket.IO or Pusher.
- Maps. Mapbox.

## High level components

1. Customer web app. Browse, order, pay, track.
2. Restaurant dashboard. Accept orders, manage menu, settle.
3. Courier app. Pickup and deliver.
4. API service.
5. Order state machine.

## Data model

- restaurant. id, name, phone, city, lat, lng, verified_at, open_hours.
- menu_item. id, restaurant_id, name, price, category, available.
- order. id, customer_phone, restaurant_id, total, service_fee, delivery_fee, status.
- order_item. id, order_id, menu_item_id, qty, price.
- delivery. id, order_id, courier_id, status, picked_at, delivered_at.
- payment. id, order_id, amount, provider, provider_ref, status.
- review. id, order_id, stars, body.

## API surface

- GET /restaurants?city=, GET /restaurants/:id/menu
- POST /orders, POST /payments/initiate
- PATCH /orders/:id/status
- POST /webhooks/payment
- POST /deliveries/assign

## Order lifecycle

Order flows through states: created, paid, accepted, preparing, ready, assigned, picked, delivered, closed. Each transition is validated and recorded. Restaurants see new orders in a tablet view. Couriers get assigned when the kitchen marks ready.

## Deployment

Standard managed stack. Payment provider sandbox for the MVP. Mapbox free tier.

## Scaling considerations

Lunch and dinner bursts. Precompute restaurant lists by city. Cache menus with revalidation. Keep the order state machine strictly serialized per order to avoid race conditions.
