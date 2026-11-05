# SkillToWork Local Architecture

## Shape
A worker-facing client, employer portal, and moderation console use an API and relational database. Listing search and candidate applications are separate modules.

## Main modules and records
- Worker profiles, skill tags, work samples, and reference consent.
- Employer accounts and job listings.
- Applications, shortlist status, and employer messages.
- Reports, moderation decisions, and listing history.

## Main flow
An employer publishes a reviewed listing. A worker applies with a relevant profile. The employer updates status and contacts shortlisted applicants through the platform.

## Operations and safeguards
Do not expose private documents publicly. Verify employers, provide report/block controls, and avoid requiring applicants to pay to access jobs.
