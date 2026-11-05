# Jirani Delivery Desk Architecture

## Shape
Merchant and courier interfaces share an API and database. A dispatch module assigns tasks; notifications can begin as in-app updates or a classroom mock.

## Main modules and records
- Merchant accounts, orders, and delivery areas.
- Courier profiles, availability, and service area.
- Delivery assignments and status events.
- Customer contact preference and daily fee summary.

## Main flow
A merchant creates a delivery request. A dispatcher assigns an available courier. The courier updates pickup and delivery status, and the merchant can review completed work.

## Operations and safeguards
Limit customer details to the assigned courier and merchant. Avoid background GPS tracking. Restrict assignment changes and log status history for dispute review.
