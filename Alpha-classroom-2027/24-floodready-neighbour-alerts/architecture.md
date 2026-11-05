# FloodReady Neighbour Alerts Architecture

## Shape
A community client, moderator dashboard, and responder view use one API and database. Notifications are an optional service behind an adapter.

## Main modules and records
- Reports: area, type, timestamp, description, optional media.
- Moderation status, reviewer, and verification note.
- Alerts and route conditions with source and expiry.
- Responder organization and update history.

## Main flow
A resident submits a report. A moderator reviews it and marks its status. A verified update is shown with its source and time and can be shared with participating responders.

## Operations and safeguards
Use coarse location and expire time-sensitive alerts. Protect submitter identity, moderate harmful content, and clearly label official information versus community reports.
