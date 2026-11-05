# Community Health Referral Tracker Architecture

## Shape
A field-worker client and facility dashboard connect to a secured API and database. Keep referral handling and aggregate reporting as separate modules.

## Main modules and records
- Organization users and assigned roles.
- Facility directory and service contacts.
- Referral: pseudonymous client reference, category, consent, destination, status.
- Follow-up tasks and de-identified aggregate counts.

## Main flow
A worker creates a referral with consent. The receiving facility updates its status for authorized users. The worker follows up and closes the referral; reports use aggregate counts only.

## Operations and safeguards
Use least-privilege access, audit sensitive reads and writes, and avoid names unless the organization requires them. Define retention and deletion rules and never publish individual referral data.
