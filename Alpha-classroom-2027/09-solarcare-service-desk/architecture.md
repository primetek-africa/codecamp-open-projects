# SolarCare Service Desk Architecture

## Shape
A responsive client connects to an API and relational database. A technician directory, service request workflow, and administration area are modules in one application.

## Main modules and records
- Customer and technician accounts, services, and service areas.
- Service requests: system type, issue, preferred slot, status.
- Appointments and job updates.
- Completed service record: work summary, parts, date, customer feedback.

## Main flow
The customer submits a request. A technician accepts and schedules it, then updates job status and records work completed. The customer can close the request or report an issue.

## Operations and safeguards
Restrict customer contact and job data to involved parties. Provide a report flow and administrator review. Do not collect equipment credentials or expose a customer's exact address publicly.
