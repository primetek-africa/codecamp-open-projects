# Community Water Point Monitor Architecture

## Shape
Use a responsive client, one API, and a relational database. Add a map provider only if users find it useful; an area-based list is enough for the first pilot.

## Main modules and records
- Water points: area, type, access notes, status, last verified.
- Reports: submitter, issue, approximate location, date, review state.
- Repairs and status history: assigned team, action, timestamps.
- Vendors and service availability.

## Main flow
A resident submits a report. A moderator verifies it or requests more information. A water manager posts a repair update and the public listing displays the verified status and update date.

## Operations and safeguards
Show source and freshness of status. Restrict exact contact and contributor details. Rate-limit reports and provide a correction path for inaccurate information.
