# CatchConnect Fish Market Architecture

## Shape
A mobile-first web client communicates with one API and database. Add notification delivery as an optional service after the core order flow works.

## Main modules and records
- Accounts with fisher or buyer roles.
- Landing sites and catch listings: species, estimated quantity, landing time, price.
- Orders: requested amount, pickup details, confirmation, status.
- Buyer and seller notification preferences.

## Main flow
The fisher posts an estimated catch. Buyers request quantities. The fisher confirms final availability and pickup, then marks the order completed or unavailable.

## Operations and safeguards
Validate listing ownership and make estimates explicit. Avoid publishing sensitive personal details. Keep status updates auditable and use area-level rather than continuous location tracking.
