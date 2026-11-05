# DukaBooks Merchant Ledger Architecture

## Shape
A private merchant interface connects to an API and relational database. Sales, expenses, customer balances, and reporting can be modules in a single app.

## Main modules and records
- Shop accounts and authorized users.
- Transactions: type, amount, date, optional category, note.
- Customers and credit entries with balance history.
- Daily and monthly summaries and data export.

## Main flow
A shop owner records sales, expenses, and credit repayments. The app updates totals and shows a date-based summary. Corrections create a traceable adjustment rather than silently deleting a transaction.

## Operations and safeguards
Keep each shop's ledger private, secure login, and support data export and deletion. Validate amounts and currency, back up records, and avoid handling payment credentials.
