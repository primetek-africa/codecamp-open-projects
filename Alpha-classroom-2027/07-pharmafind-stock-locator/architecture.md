# PharmaFind Architecture

## Overview

PharmaFind is a geospatial search product. The main architectural concerns are fast nearby queries, keeping stock updates cheap for pharmacies, and avoiding stale data.

## Suggested stack

- Front end. Next.js with server rendering on search results pages for SEO.
- API. Node with Fastify or Django.
- Database. PostgreSQL with PostGIS or a geohashing scheme for nearby queries.
- Search. PostgreSQL full text search for product names. Add Meilisearch or Typesense if latency becomes a problem.
- Cache. Redis for popular medicine queries.
- Maps. Mapbox or OpenStreetMap tiles with Leaflet.
- SMS. Twilio or Africa's Talking.

## High level components

1. Patient web app. Map, search, reservation.
2. Pharmacy portal. Bulk stock update, orders, reservations.
3. Admin panel. Pharmacy verification, moderation.
4. API service.
5. Stock import worker. Processes CSV uploads.

## Data model

- pharmacy. id, name, license_no, phone, location (point), opening_hours, verified_at.
- staff. id, pharmacy_id, phone, role.
- product. id, name, generic_name, active_ingredient, dosage_form, pack_size.
- stock. id, pharmacy_id, product_id, quantity, unit_price, updated_at.
- reservation. id, pharmacy_id, product_id, user_id, qty, status, expires_at.
- message. id, reservation_id, author_role, body, created_at.

## API surface

- GET /search?q=amoxicillin&lat=-1.28&lng=36.82&radius_km=3
- GET /pharmacies/:id
- POST /stock/bulk with a CSV
- POST /reservations, POST /reservations/:id/confirm
- POST /messages

## Search and ranking

Rank by distance, stock freshness and verified status. Boost pharmacies that have updated stock in the last 24 hours. Filter out pharmacies that have not updated in 14 days unless the user explicitly asks for all results.

## Deployment

Next.js on Vercel. API and workers on Render. Managed PostgreSQL with PostGIS. Mapbox free tier covers the MVP. Add a CDN in front of static assets.

## Scaling considerations

Full text search is cheap at 10k pharmacies. Beyond that, move to Meilisearch or Typesense. Keep a materialized view for popular medicines in a city. Index pharmacy on location with GIST and on verified_at.
