# ChamaLink Architecture

## Overview

ChamaLink is a shared ledger product. The core design question is correctness of the balance per member and per group at any point in time. The second concern is low bandwidth usage because many groups meet in semi-rural locations.

## Suggested stack

- Front end. Nuxt or Next.js as a PWA so group members on cheap Android phones can install it on the home screen without a Play Store download.
- API. Laravel or Node with Express or Fastify. Laravel is a good fit because the domain is ledger heavy and migrations stay simple.
- Database. MySQL or PostgreSQL. Use decimal columns for money. Never floats.
- Background jobs. Laravel Queues or BullMQ for reminders and PDF export.
- SMS. Africa's Talking for East Africa, Twilio or Termii for West Africa.
- Auth. Phone OTP via the SMS provider. JWT access token with refresh.
- Storage. Cloudinary for member photos or an S3 compatible bucket.

Other valid stacks include Vue 3 with Pinia and an Adonis API, or React with a Django REST API.

## High level components

1. Web app and PWA. Treasurer view, member view and NGO view.
2. API service. Auth, groups, members, contributions, loans, meetings, reports.
3. Jobs worker. Reminders, statement generation.
4. SMS gateway adapter. One module that can swap providers.
5. Admin panel. For platform support and NGO oversight.

## Data model

Core entities and the key fields.

- user. id, phone, name, role, locale.
- group. id, name, currency, contribution_amount, frequency, treasurer_id, status.
- membership. id, group_id, user_id, role, joined_at, exited_at.
- contribution. id, group_id, member_id, amount, paid_on, meeting_id, reference.
- loan. id, group_id, member_id, principal, interest_rate, term_months, status, approved_at.
- repayment. id, loan_id, amount, paid_on, reference.
- meeting. id, group_id, held_on, agenda, attendance_json, decisions.
- statement_export. id, group_id, period_from, period_to, pdf_url, created_by.

## API surface

REST endpoints grouped by resource. Example routes:

- POST /auth/otp and /auth/verify
- POST /groups, GET /groups/:id, POST /groups/:id/members
- POST /groups/:id/contributions, GET /groups/:id/ledger
- POST /loans, POST /loans/:id/approve, POST /loans/:id/repayments
- POST /meetings, GET /meetings/:id
- POST /exports/statement

Prefer GraphQL only if the team is already comfortable with it, otherwise REST is faster to ship.

## Background jobs

- Meeting reminder job scheduled 24 hours and 1 hour before each meeting.
- Contribution confirmation job that sends an SMS after each entry.
- Statement export job that generates the PDF and emails or links it.
- Nightly ledger integrity check that sums contributions and loans per member.

## Integrations

- Africa's Talking SMS or Twilio for notifications.
- Mobile money reference capture only. Do not pull funds in the MVP.
- Later, M-Pesa Daraja or Flutterwave collection endpoints.

## Deployment

Single region deployment is enough. Hostinger, Railway, Render, Fly.io or DigitalOcean App Platform are all suitable. Use managed MySQL or PostgreSQL. Keep secrets in the platform secret store. Enable daily automated database backups.

## Scaling considerations

The ledger is write heavy at meeting time but otherwise quiet. Add a composite index on contribution(group_id, paid_on) and loan(group_id, status). Use read replicas only if the NGO oversight view becomes slow. For PWA offline, cache the group ledger with a stale while revalidate strategy.
