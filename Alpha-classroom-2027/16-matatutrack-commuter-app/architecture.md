# MatatuTrack Architecture

## Overview

MatatuTrack combines a static route catalogue with a stream of crowdsourced reports. The architecture must be cheap because many operators will not pay.

## Suggested stack

- Mobile. React Native or Flutter.
- Operator dashboard. Nuxt or Next.js.
- API. Node with Fastify or Django.
- Database. PostgreSQL with PostGIS.
- Cache. Redis for live position summaries.
- Maps. Mapbox or OpenStreetMap with Leaflet.
- Queue. BullMQ or Celery.

## High level components

1. Commuter app. Browse routes, see next vehicle estimate, report a sighting.
2. Conductor app. Fare entry, start and end shift.
3. Operator dashboard. SACCO view.
4. Public open data API.
5. API service.
6. Estimator service. Computes next arrival estimate from recent sightings.

## Data model

- operator. id, name, type, city.
- route. id, operator_id, name, stops_json, fare_default, geojson.
- stop. id, name, lat, lng, city.
- vehicle. id, operator_id, plate, route_id, status.
- shift. id, vehicle_id, conductor_id, started_at, ended_at, total_fares.
- fare_entry. id, shift_id, amount, created_at.
- sighting. id, route_id, stop_id, reporter_role, reported_at.
- rider_report. anonymous aggregate used only for analytics.

## API surface

- GET /routes, GET /routes/:id/next
- POST /sightings
- POST /shifts, POST /shifts/:id/fares
- GET /operators/:id/summary
- GET /open/routes.geojson

## Estimator

For each route, a service maintains a sliding window of sightings. It computes the typical time between sightings at each stop. Next vehicle estimate is the median interval added to the last sighting time, capped at 30 minutes. Lightweight and explainable.

## Deployment

API on Render or Fly.io. Managed PostgreSQL with PostGIS. Static dashboard on Vercel. Mobile via Expo EAS.

## Scaling considerations

Sighting write volume can spike at rush hour. Batch inserts with a short buffer. Shard the estimator by city. Open data endpoints serve cached GeoJSON through a CDN.
