# NearbyMeds Stock Finder Architecture

## Shape
Use a responsive customer client and pharmacy dashboard backed by an API and relational database. Product search and pharmacy updates are separate modules in one app.

## Main modules and records
- Pharmacies, staff users, and service areas.
- Product catalogue entries with pharmacy-specific stock status.
- Stock update actor and timestamp.
- Reservation enquiries and response status.

## Main flow
A pharmacy staff member updates a stock state. A customer searches by product and area, checks the update time, and sends an enquiry. The pharmacy confirms or declines directly.

## Operations and safeguards
Limit product categories to the pilot and verify pharmacy participation. Make the data source and update time visible. Protect staff accounts and do not collect prescriptions or provide clinical guidance.
