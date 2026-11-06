# RentalVerify Architecture

## Overview

RentalVerify is a marketplace plus payment ledger plus workflow app. The core architectural concern is verification trust and the rent ledger per unit.

## Suggested stack

- Front end. Nuxt or Next.js.
- API. Laravel or Node.
- Database. MySQL.
- Payments. Paystack or M-Pesa.
- Storage. Cloudinary.
- SMS. Africa's Talking.

## High level components

1. Landlord dashboard. Properties, tenants, payments.
2. Tenant portal. Apply, pay, request maintenance.
3. Admin panel. Verification, dispute mediation.
4. API service.
5. Payment service.

## Data model

- landlord. id, name, phone, id_document, verified_at.
- property. id, landlord_id, title, address, lat, lng, type, bedrooms.
- unit. id, property_id, name, rent_amount.
- tenant. id, user_id, phone, employer_ref, previous_landlord_ref, verified_at.
- lease. id, unit_id, tenant_id, start_date, end_date, deposit, rent_amount.
- rent_schedule_item. id, lease_id, due_on, amount, paid_amount, status.
- payment. id, lease_id, item_id, amount, method, reference, received_at.
- maintenance_request. id, unit_id, body, photo_url, status.

## API surface

- POST /landlords/apply
- POST /properties
- POST /applications
- POST /leases
- POST /payments
- POST /maintenance-requests

## Deployment

Standard managed stack. Payment provider sandbox for the MVP. Daily reconciliation cron.

## Scaling considerations

Rent collection happens in a short window around the first of the month. Precompute due today lists. Add indexes on rent_schedule_item(due_on, status).
