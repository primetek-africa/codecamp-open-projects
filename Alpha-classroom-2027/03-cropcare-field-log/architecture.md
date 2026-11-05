# CropCare Field Log Architecture

## Shape
A mobile-first client uses an API backed by a database and private image storage. The adviser content workflow is a separate module within the same application.

## Main modules and records
- Users, plots, and crops.
- Observations: symptom description, date, optional photo reference, consent.
- Guidance cards: author, source, review date, crop and region tags.
- Referrals and reminders: assigned adviser, status, due date.

## Main flow
A farmer logs an observation and requests a review. An adviser views the report, replies with reviewed information, or refers the farmer to a local service.

## Operations and safeguards
Keep uploaded photos private and enforce record ownership. Collect only needed location detail, support deletion requests, and show that guidance is informational, not a diagnosis.
