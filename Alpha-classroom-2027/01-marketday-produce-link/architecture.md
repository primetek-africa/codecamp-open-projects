# MarketDay Produce Link Architecture

## Shape
A responsive client talks to a single application API and relational database. A modular monolith is sufficient for a classroom pilot.

## Main modules and records
- Accounts and roles: farmer, buyer, administrator.
- Listings: crop, unit, quantity, price, harvest date, area, status.
- Orders: buyer request, accepted quantity, pickup details, status, timestamps.

## Main flow
Farmers publish listings. Buyers filter by crop and area and request an order. The farmer accepts or declines it, then both sides see the pickup and status.

## Operations and safeguards
Validate listing quantities and ownership on the server. Add role-based access, pagination, backups, and administrator review of reports. Do not store card details; use a payment provider only if a later pilot requires it.
