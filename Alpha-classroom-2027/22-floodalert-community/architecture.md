# FloodAlert Architecture

## Overview

FloodAlert is a report plus map plus alert app. It must work for anonymous users and avoid exposing personal information.

## Suggested stack

- Front end. Nuxt or Next.js as a PWA.
- API. Node or Django.
- Database. PostgreSQL with PostGIS.
- Maps. Mapbox or Leaflet with OpenStreetMap.
- SMS. Africa's Talking.
- Jobs. node-cron or Celery.

## High level components

1. Public PWA. Report, map, subscribe.
2. Responder dashboard.
3. Admin panel for moderation.
4. API service.
5. Alert worker.

## Data model

- report. id, reporter_token, category, severity, lat, lng, photo_url, note, created_at, status.
- ward. id, name, city, geojson.
- subscription. id, phone, ward_id, created_at.
- alert. id, ward_id, body, created_at, sent_count.
- moderator. id, name, email, role.

## API surface

- POST /reports
- GET /reports?bbox=
- POST /subscriptions
- POST /alerts (moderator only)
- POST /reports/:id/verify

## Alerts

An alert worker watches new verified reports and groups them by ward. If a ward has 3 verified reports in the last 30 minutes, send an alert to subscribers in that ward with a 60 minute cooldown.

## Deployment

Standard managed stack. Managed PostgreSQL with PostGIS. Mapbox free tier covers a small city. CDN in front of the public site.

## Scaling considerations

Report volume spikes during storms. Keep the write path simple. Precompute the active alerts map every 30 seconds and serve from Redis or a view.
