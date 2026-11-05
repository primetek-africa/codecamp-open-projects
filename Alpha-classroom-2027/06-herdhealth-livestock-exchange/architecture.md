# HerdHealth & Livestock Exchange Architecture

## Shape
Use a responsive client, API, relational database, and private media storage only if listing photos are needed.

## Main modules and records
- Accounts and service providers, including verification state.
- Animals and care records: event type, date, note, provider.
- Listings: animal reference, asking terms, area, availability.
- Enquiries with seller, buyer, status, and contact preference.

## Main flow
A keeper records an animal and care events, then optionally publishes a sale listing. A buyer sends an enquiry and the seller responds outside the platform or through a basic message thread.

## Operations and safeguards
Restrict animal records to their owner and authorized providers. Minimize personal and precise location data. Label provider verification accurately and do not provide automated veterinary decisions.
