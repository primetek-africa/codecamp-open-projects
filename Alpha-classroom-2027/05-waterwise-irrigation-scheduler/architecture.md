# WaterWise Irrigation Scheduler Architecture

## Shape
A mobile-friendly client connects to an API and relational database. Scheduling and group administration can remain modules in one deployable application.

## Main modules and records
- Groups, users, plots, and shared resources.
- Slot requests: resource, requested period, duration, plot, status.
- Schedule entries: approved slot, responsible user, change history.
- Usage records and reminder preferences.

## Main flow
Members request slots. A manager reviews conflicts and publishes a schedule. Users record usage or report a missed slot; the group sees the updated timetable.

## Operations and safeguards
Enforce group membership and manager roles. Record schedule changes and timezone consistently. Do not present user-entered usage as a calibrated water measurement.
