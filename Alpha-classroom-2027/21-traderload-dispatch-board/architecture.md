# TraderLoad Dispatch Board Architecture

## Shape
Use a responsive client, API, and relational database. Profiles, load listings, matching, and administration are modular features of a single deployable service.

## Main modules and records
- Trader and driver accounts, vehicles, and capacity.
- Loads: goods category, size, route, pickup date, status.
- Trip offers and accepted matches.
- Delivery confirmation, commission, and dispute report.

## Main flow
A trader posts a load. Drivers filter loads and offer a trip. The trader accepts an offer, and participants record pickup and delivery status.

## Operations and safeguards
Restrict contact data to matched participants. Keep a status history, provide a report path, and use approximate route areas instead of continuous location surveillance.
