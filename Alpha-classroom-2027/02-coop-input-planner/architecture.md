# Co-op Input Planner Architecture

## Shape
Use a responsive client, one API, and a relational database. Keep catalogue, cooperative, and order logic as modules in one application.

## Main modules and records
- Cooperatives and memberships: member role, status, and contact preference.
- Catalogue items: supplier, description, unit, price, season, availability.
- Group orders and order lines: deadline, requested quantity, approval, status.
- Payments: amount and status only, with no payment credentials.

## Main flow
A manager opens a seasonal order. Members submit quantities. The app totals demand, supports review, and generates a supplier-ready summary.

## Operations and safeguards
Enforce cooperative membership and manager permissions on every write. Preserve edits to order quantities in a simple audit history. Export only the information the supplier needs and back up the database.
