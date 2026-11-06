# BodaGo Architecture

## Overview

BodaGo is a classic two sided realtime marketplace. The main architectural challenges are matching, live location streaming and payment settlement.

## Suggested stack

- Mobile. React Native with Expo or Flutter. Separate rider and passenger builds that share a core package.
- API. Node with Fastify and Socket.IO or NestJS with websockets.
- Database. PostgreSQL with PostGIS for geo queries.
- Cache. Redis for live rider locations and pub sub.
- Maps. Mapbox or Google Maps SDK.
- Payments. M-Pesa Daraja, Paystack or Flutterwave.
- Queue. BullMQ.

## High level components

1. Passenger app. Request a ride, live trip, pay.
2. Rider app. Online toggle, incoming request, navigation, wallet.
3. API service.
4. Dispatch service. Receives requests and finds the closest available rider.
5. Location service. Receives rider location pings, keeps in Redis, broadcasts to the passenger during a trip.
6. Payment service.
7. Admin panel.

## Data model

- rider. id, user_id, national_id, bike_plate, verified_at, status.
- passenger. id, user_id, favorite_places_json.
- trip_request. id, passenger_id, pickup (point), dropoff (point), distance_km, fare_estimate, status, created_at.
- trip. id, request_id, rider_id, started_at, ended_at, actual_fare, commission.
- rider_wallet. id, rider_id, balance, pending_amount.
- payout. id, rider_id, amount, period_from, period_to, status.
- rating. id, trip_id, by, stars, body.

## API surface

- POST /auth/otp
- POST /riders/apply
- POST /requests, GET /requests/:id/status
- WS /driver/stream for location pings
- POST /trips/:id/complete, POST /trips/:id/pay
- POST /webhooks/payment

## Matching

The dispatch service holds a geo index of online riders in Redis using geohashing. On a new request, it queries riders within a radius, filters by vehicle type and rating, and sends the request to the top candidate with a short timeout. On timeout, move to the next candidate. Record a dispatch log.

## Location streaming

Rider app pings location every 3 seconds while a trip is active and every 15 seconds while idle. The passenger during a trip subscribes via a per trip socket channel and sees the rider position with a short smoothing interpolation on the client.

## Payments

Fare is estimated at request time. The passenger can pay by cash or by mobile money. Cash is recorded by the rider. Mobile money is initiated via the provider and reconciled by webhook. Commission is held back and settled to the platform.

## Deployment

API and sockets on Fly.io, Render or AWS ECS. Managed PostgreSQL with PostGIS. Managed Redis. Mobile builds via Expo EAS.

## Scaling considerations

Location streaming is the main cost. Batch pings, drop redundant ones on the server. Shard the geo index by city. Use regional deployments if the app grows to multiple countries.
