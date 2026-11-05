# MatatuSeat Booking Architecture

## Shape
Passenger search and operator management use an API and relational database. Route, departure inventory, and booking logic live in one application.

## Main modules and records
- Operators and route definitions.
- Departures with time, capacity, and current availability.
- Bookings with passenger contact, requested seats, and status.
- Booking references, cancellation, and payment status.

## Main flow
An operator publishes a departure and available seats. A passenger submits a request. The operator confirms or declines; only confirmed bookings reduce visible availability.

## Operations and safeguards
Use server-side transactions to prevent overselling. Make operator confirmation status clear, protect passenger contact details, and allow cancellation and correction.
