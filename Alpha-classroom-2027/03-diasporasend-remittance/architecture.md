# DiasporaSend Architecture

## Overview

DiasporaSend is a read heavy comparison engine with a small write path for transfer tracking. The main challenge is keeping provider data fresh across many corridors without overloading provider sites.

## Suggested stack

- Front end. Next.js with server rendering for SEO. Corridor pages like /send/gbp-to-kes rank well in search.
- API. Node with Fastify or Django with DRF. Django is attractive because of good scraping and scheduling tooling.
- Database. PostgreSQL. Use timescale extension only if historical rate charts become important.
- Cache. Redis for hot corridor queries.
- Scraper. Python with Playwright for provider sites that need a browser. Cron orchestration via GitHub Actions or a scheduled Render job.
- Email. Resend. SMS. Twilio or Africa's Talking.

## High level components

1. Web app. Compare page, provider profile, tracker, alert settings.
2. Public API. Rate query, provider list, transfer endpoints.
3. Rate collector. Scheduled job per provider per corridor.
4. Tracker service. Stores a user transfer and polls provider status where supported.
5. Alert service. Reads user thresholds and sends alerts.

## Data model

- user. id, email, phone, country, preferred_corridor.
- provider. id, name, website, licensing_jurisdiction, payout_methods_json.
- corridor. id, send_country, receive_country, send_currency, receive_currency.
- rate_sample. id, provider_id, corridor_id, send_amount, receive_amount, fee, sampled_at.
- transfer. id, user_id, provider_id, corridor_id, send_amount, reference, status, created_at.
- alert. id, user_id, corridor_id, threshold_rate, channel.
- review. id, user_id, provider_id, corridor_id, stars, body, created_at.

## API surface

- GET /compare?send=GBP&receive=KES&amount=100
- GET /providers, GET /providers/:id
- POST /transfers, GET /transfers/:id
- POST /alerts, GET /alerts
- POST /reviews

## Rate collection

Each provider has an adapter. Where the provider exposes a public rate API, call it directly on a schedule. Where only the site exists, use Playwright to pull the quote for the chosen amount. Store every sample in rate_sample so charts and sorting stay accurate. Respect robots.txt and rate limit to be a polite client.

## Alerts

A cron job runs every 15 minutes and compares the latest rate_sample for each watched corridor against each user alert. Trigger a single alert per threshold crossing to avoid spam.

## Deployment

Next.js on Vercel. API and workers on Render or Fly.io. Managed PostgreSQL and Redis. Store scraping artifacts in S3 compatible storage for audit.

## Scaling considerations

Rate collection can grow quickly. Shard scrape jobs by provider and run them in parallel. Cache corridor compare queries in Redis for 30 to 60 seconds. If search volume is very high, serve compare results from the edge with revalidation.
