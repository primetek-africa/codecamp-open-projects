# KopaCredit Architecture

## Overview

KopaCredit is a multi tenant SaaS for lenders. Each lender organization has its own staff, borrowers and portfolio. Data isolation is the most important architectural requirement.

## Suggested stack

- Front end. Nuxt or Next.js for the staff dashboard. A thin borrower PWA for application and balance view.
- API. Laravel or Django. Both come with strong permission tooling for multi tenant RBAC.
- Database. PostgreSQL. Decimal columns for money. Row level tenant column on every table.
- Queue. Laravel Queues or Celery for reminders and nightly jobs.
- SMS. Africa's Talking or Termii.
- Payments. M-Pesa Daraja for Kenya, Paystack or Flutterwave for West Africa.

## High level components

1. Staff dashboard. Loans, borrowers, portfolio, settings.
2. Borrower PWA. Apply, see balance and schedule.
3. API service. Multi tenant with a tenant_id scope on every query.
4. Jobs worker. Reminders, overdue flagging, portfolio at risk rollup.
5. Mobile money webhook receiver. Reconciles inbound payments against loans.

## Data model

- tenant. id, name, country, timezone, currency.
- staff. id, tenant_id, phone, role.
- borrower. id, tenant_id, name, phone, national_id, kyc_status.
- loan_product. id, tenant_id, name, min_amount, max_amount, interest_rate, term_days, fee.
- loan_application. id, tenant_id, borrower_id, product_id, amount, purpose, status.
- loan. id, tenant_id, application_id, principal, interest, fees, disbursed_at, maturity_at, status.
- repayment_schedule_item. id, loan_id, due_on, amount, paid_amount, status.
- repayment. id, loan_id, amount, method, reference, received_at.
- par_snapshot. id, tenant_id, as_of, par_30, par_60, par_90.

## API surface

- POST /auth/login
- GET /borrowers, POST /borrowers
- POST /applications, POST /applications/:id/approve
- POST /loans/:id/disburse, POST /loans/:id/repayments
- GET /portfolio/overview, GET /portfolio/par
- POST /webhooks/mobile-money

## Scoring

A rule engine that assigns points for declared income, dependents, previous repayment history, and optional ShopBook sales integration. Keep the rule set in configuration so lenders can tune it. Store the input and output of each scoring run for audit.

## Background jobs

- Daily overdue check that updates schedule item status.
- Daily PAR snapshot per tenant.
- Reminder jobs at T minus 3 days, T minus 1 day, and on due date.
- Monthly regulator export for countries that require it.

## Deployment

Multi tenant single database with a tenant_id column is simpler than schema per tenant and good enough at the MVP scale. Host API on Render or Fly.io. Managed PostgreSQL with point in time recovery. Keep an encrypted daily backup off site.

## Scaling considerations

The hot path is schedule queries. Index schedule_item(tenant_id, due_on, status). Portfolio rollup should be precomputed nightly. For large lenders, add read replicas and move PAR calculation to a scheduled job rather than ad hoc.
