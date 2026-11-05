# WardWatch Civic Requests Architecture

## Shape
A resident reporting client, agency dashboard, and moderation console use an API and relational database. Public issue summaries are generated separately from private contact details.

## Main modules and records
- Ward and issue category directory.
- Reports with optional private reporter contact and attachments.
- Status events and agency responses.
- Moderation decisions and aggregated public metrics.

## Main flow
A resident submits a report. A moderator checks it for personal or harmful content. The participating agency updates status; the public view shows a ward-level summary and timeline.

## Operations and safeguards
Separate reporter identity from public report data. Use role-based access and audit updates. Add report removal and correction workflows and label agency participation accurately.
