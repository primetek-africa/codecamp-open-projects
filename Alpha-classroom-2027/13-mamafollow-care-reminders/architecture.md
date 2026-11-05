# MamaFollow Care Reminders Architecture

## Shape
A staff dashboard and a minimal patient preference flow connect to a secured API and database. Notifications are queued through an adapter so a classroom demo can use mock delivery.

## Main modules and records
- Clinic staff and role permissions.
- Patient contact and consent state.
- Visit schedule and reminder tasks.
- Delivery attempts, opt-out, and follow-up status.

## Main flow
Staff record a planned visit. The reminder service checks consent and preference, queues a clinic-approved message, and records the result. Staff use a follow-up list for missed appointments.

## Operations and safeguards
Store the minimum personal data, encrypt traffic, restrict access, and set a retention policy. Prevent sensitive health details from appearing in lock-screen messages or public logs.
