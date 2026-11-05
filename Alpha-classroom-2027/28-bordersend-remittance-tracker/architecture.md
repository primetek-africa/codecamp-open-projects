# BorderSend Remittance Tracker Architecture

## Shape
A public provider directory and private sender workspace use an API and relational database. Keep quote management and transfer tracking separate; no payment rails are included.

## Main modules and records
- Provider profiles and verification state.
- Quote: currencies, fee, rate, effective time, source.
- Private transfer record: provider, reference, amount, status, receipt.
- Referral attribution and support request.

## Main flow
A provider updates its public quote with a timestamp. A sender compares listings, then records a transfer reference obtained directly from the provider. The sender updates status or attaches a receipt.

## Operations and safeguards
Use a licensed provider for any future financial integration and review applicable local regulations first. Make quote freshness and data source prominent; secure private references and receipts.
