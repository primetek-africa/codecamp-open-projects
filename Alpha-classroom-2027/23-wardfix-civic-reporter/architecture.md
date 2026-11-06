# WardFix Architecture

## Overview

WardFix is a report plus workflow app with a public transparency layer. The main architectural concern is privacy of reporters versus transparency of resolution.

## Suggested stack

- Front end. Next.js or Nuxt. Static public pages per ward for SEO.
- API. Laravel or Node.
- Database. PostgreSQL with PostGIS.
- Email. Resend or Mailgun for agency notifications.
- Storage. Cloudinary for photos.
- Map. Mapbox or Leaflet.

## High level components

1. Public PWA. Report, browse ward.
2. Agency portal. Inbox, status updates.
3. Councillor view.
4. API service.
5. Notification worker.

## Data model

- report. id, reporter_id, category, title, body, photo_url, lat, lng, ward_id, status, created_at.
- ward. id, name, city, geojson.
- agency. id, name, categories_json, email.
- status_update. id, report_id, author_id, status, body, created_at.
- subscriber. id, phone_or_email, ward_id, categories_json.

## API surface

- POST /reports
- GET /wards/:id
- POST /reports/:id/updates
- GET /public/wards/:id/stats
- POST /webhooks/agency-reply

## Deployment

Standard managed stack. CDN in front of public pages. Agency inbox can be a simple email integration before adding full identity for agencies.

## Scaling considerations

Reports scale linearly with population. Add indexes on report(ward_id, status, created_at). Precompute ward stats daily.
