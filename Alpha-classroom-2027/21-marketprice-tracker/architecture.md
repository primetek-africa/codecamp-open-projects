# MarketPrice Tracker Architecture

## Overview

MarketPrice is a data collection and distribution product. The architecture is simple but needs attention to data quality and low cost SMS distribution.

## Suggested stack

- Front end. Vue or Nuxt for the public site and admin. Field agent entry as a PWA.
- API. Node or Django.
- Database. PostgreSQL. A time series table for prices.
- Charts. Chart.js or ECharts.
- SMS. Africa's Talking or Termii.
- Jobs. node-cron or Celery.

## High level components

1. Agent PWA. Entry form by market and product.
2. Public site. Browse markets, products, charts.
3. Admin panel. Verification, outlier flagging, agent management.
4. API service.
5. SMS sender.
6. Weekly report generator.

## Data model

- region. id, country, name.
- market. id, region_id, name, lat, lng, type.
- product. id, name, unit.
- price_entry. id, market_id, product_id, entered_by, price, unit_qty, entered_for_date, verified_by.
- agent. id, phone, region_id, verified_at.
- alert. id, user_phone, market_id, product_id, threshold, last_sent_at.

## API surface

- POST /entries with batches for a day
- GET /markets, GET /products
- GET /markets/:id/prices?product=maize
- POST /alerts, DELETE /alerts/:id

## Data quality

New entries that are more than 20 percent above or below the 7 day median of the same product at the same market get flagged for verification before being published. Keep the raw entry and a published flag so that nothing is deleted silently.

## Deployment

Standard managed stack. Managed PostgreSQL. Scheduled jobs for alerts and reports. Public site can go behind a CDN.

## Scaling considerations

Time series rows grow linearly with markets times products times days. Expected size is small for the MVP. Add indexes on price_entry(market_id, product_id, entered_for_date). Partition by year once the table passes 50 million rows.
