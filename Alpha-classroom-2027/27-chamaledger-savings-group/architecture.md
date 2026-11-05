# ChamaLedger Savings Group Architecture

## Shape
Member and treasurer views use one API backed by a relational database. Group administration, ledger entry, and statements are modules in one application.

## Main modules and records
- Groups, members, roles, and approval rules.
- Contributions, loans, repayments, and adjustments.
- Meeting records and approvals.
- Member statements and export jobs.

## Main flow
A treasurer records a contribution or loan repayment. A second authorized member reviews or approves it when group rules require. Members see only their group's ledger and agreed balances.

## Operations and safeguards
Require group-level access checks and preserve an audit trail. Allow corrections with reason and reviewer. Never store payment PINs or hold group funds.
