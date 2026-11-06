# ShopBook Architecture

## Overview

ShopBook is a mobile first PWA with a strong offline requirement. Many shops have intermittent connectivity and the owner cannot stop a sale to wait for the network. The architecture is organized around a local first store that syncs to a central API.

## Suggested stack

- Front end. React with Vite as a PWA, or Nuxt 3. Use Workbox for service worker, IndexedDB via Dexie for offline storage.
- API. Node with Fastify or Laravel. Laravel is fine if the team is more comfortable there.
- Database. MySQL. Decimal columns for money.
- Realtime. Not required for MVP.
- Payments. Paystack or M-Pesa Daraja for later automatic reconciliation.
- SMS. Africa's Talking or Termii for receipts.

## High level components

1. PWA shell. Pages for sales, catalogue, credit book, reports.
2. Local store. Dexie database with products, sales, credits.
3. Sync engine. Pushes new records to the API and pulls server changes.
4. API service. Receives sync batches, resolves conflicts, serves reports.
5. Jobs worker. Daily rollups per merchant.
6. SMS adapter. Sends receipts and alerts.

## Data model

- merchant. id, owner_phone, business_name, currency, plan, created_at.
- staff. id, merchant_id, phone, role, invited_by.
- product. id, merchant_id, name, cost_price, sell_price, stock, low_stock_threshold.
- sale. id, merchant_id, staff_id, total, paid_cash, paid_mobile, mobile_ref, customer_id, created_at, client_uuid for sync idempotency.
- sale_item. id, sale_id, product_id, qty, unit_price.
- customer. id, merchant_id, name, phone.
- credit. id, merchant_id, customer_id, sale_id, amount_outstanding, status.
- repayment. id, credit_id, amount, method, reference, received_at.
- daily_rollup. id, merchant_id, date, opening_cash, total_sales, closing_cash.

## API surface

- POST /sync/push with a batch of sales, credits, products
- GET /sync/pull with a since cursor
- POST /receipts/sms
- GET /reports/daily, /reports/products, /reports/customers
- POST /staff/invite

## Sync strategy

Each record carries a client generated UUID. The API treats the UUID as the idempotency key. Conflicts for product stock are resolved by taking the last writer with a server timestamp and recomputing stock from the sale events. Keep sync batches small to survive bad networks.

## Background jobs

- Nightly daily_rollup job that reads sales for each merchant and stores a summary row to make reports cheap.
- SMS sender queue.
- Low stock alert job.

## Deployment

Single region API with managed MySQL. Static PWA served via Vercel, Netlify or Cloudflare Pages. Use Cloudflare in front to cache the PWA shell for fast loads on slow networks.

## Scaling considerations

The main hot path is sync push. Add an index on sale(merchant_id, created_at) and on sync_log(merchant_id, client_uuid). If sync volume grows, move daily_rollup to a materialized view or warehouse.
