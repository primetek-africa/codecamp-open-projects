# SchoolPay Architecture

## Overview

SchoolPay is a multi tenant ledger for schools. Correctness of the balance per student and per class is the top priority.

## Suggested stack

- Front end. Vue or React. Staff tablet friendly. Parent view is a link only PWA.
- API. Laravel or Django.
- Database. MySQL or PostgreSQL. Decimal for money.
- Queue. Laravel Queues or Celery.
- SMS. Africa's Talking, Twilio or Termii.
- Payments. Paystack or M-Pesa Daraja.

## High level components

1. Staff dashboard. School, classes, students, payments, reports.
2. Parent balance link. Signed URL page.
3. API service.
4. Reminder worker.
5. Reconciliation worker that matches mobile money webhooks to receipts.

## Data model

- school. id, name, country, currency, timezone.
- staff. id, school_id, phone, role.
- class_level. id, school_id, name.
- student. id, school_id, class_id, name, parent_phone, parent_name.
- fee_plan. id, student_id, term, total_amount, created_at.
- installment. id, fee_plan_id, due_on, amount, paid_amount, status.
- payment. id, student_id, installment_id, amount, method, reference, received_at.

## API surface

- POST /auth/staff
- POST /students, POST /fee-plans
- POST /payments, GET /students/:id/balance
- GET /reports/arrears
- POST /webhooks/mobile-money

## Reconciliation

When a parent pays by mobile money using the school till, a webhook arrives with a reference. The reconciliation worker looks up the student by phone or by the reference encoded in the payment description. Unmatched payments go to a queue for a bursar to match manually. This avoids silent loss.

## Deployment

Standard managed stack. One tenant per school via a tenant_id column on all tables. Consider dedicated databases only for large tenants.

## Scaling considerations

Reports are the heavy query. Precompute arrears per school nightly. Add indexes on installment(student_id, due_on, status) and on payment(school_id, received_at).
