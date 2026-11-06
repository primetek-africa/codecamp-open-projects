# PlotLease Architecture

## Overview

PlotLease is a classic listings app with a booking engine and a small payment hand off. Architecture stays simple.

## Suggested stack

- Front end. Vue or Nuxt. Static landing pages for SEO.
- API. Laravel or Node.
- Database. MySQL.
- Maps. Mapbox or Leaflet.
- Payments. Paystack or Flutterwave.
- Storage. Cloudinary.

## High level components

1. Public site. Browse plots.
2. Landowner dashboard. Manage listings, enquiries.
3. Renter portal. Request, pay, review.
4. Admin panel. Moderation.
5. API service.

## Data model

- landowner. id, name, phone, id_document, verified_at.
- plot. id, landowner_id, title, lat, lng, size_sqm, allowed_uses_json, price_per_day, price_per_month, photos_json, available_from, available_to.
- enquiry. id, plot_id, renter_id, start, end, intended_use, note.
- lease. id, enquiry_id, start, end, deposit, total, status.
- payment. id, lease_id, amount, method, reference, received_at.
- review. id, lease_id, by, stars, body.

## API surface

- POST /plots
- POST /enquiries
- POST /leases
- POST /payments
- POST /reviews

## Deployment

Standard managed stack. Static pages behind a CDN for SEO. Payment provider sandbox for the MVP.

## Scaling considerations

Reads dominate. Cache plot pages aggressively. Invalidate on new photos or availability changes.
