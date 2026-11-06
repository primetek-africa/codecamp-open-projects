# SolarPay Architecture

## Overview

SolarPay is a multi tenant SaaS for installers with a strong focus on correct PAYG reconciliation.

## Suggested stack

- Front end. Next.js or Nuxt.
- API. Laravel or Node.
- Database. PostgreSQL. Decimal for money.
- Payments. M-Pesa Daraja or Flutterwave.
- SMS. Africa's Talking.
- Queue. Laravel Queues or BullMQ.

## High level components

1. Installer dashboard.
2. Customer balance page.
3. Technician view.
4. API service.
5. Reconciliation worker.
6. Reminder worker.

## Data model

- installer. id, name, country, currency, settlement_account.
- staff. id, installer_id, phone, role.
- customer. id, installer_id, phone, name, address.
- system. id, customer_id, serial_no, install_date, tariff_amount, tariff_period, total_amount, status.
- schedule_item. id, system_id, due_on, amount, paid_amount, status.
- payment. id, system_id, item_id, amount, method, reference, received_at.
- service_request. id, system_id, category, body, status.

## API surface

- POST /installers/apply
- POST /customers
- POST /systems
- POST /payments
- POST /webhooks/mobile-money
- POST /service-requests

## Reconciliation

When a customer pays, the webhook arrives with a reference that includes the system serial. The worker matches payment to system and updates the schedule. Unmatched payments go to a review queue. Scheduler flags overdue schedule items every night.

## Deployment

Standard managed stack. Payment provider sandbox for the MVP.

## Scaling considerations

Schedule row writes are the hot path at month end. Precompute installer dashboards. Partition schedule items by year once big.
