# SchoolFee StepPlan Architecture

## Shape
A caregiver portal and school administration dashboard share an API and relational database. Keep fee plans, receipts, and notification scheduling as application modules.

## Main modules and records
- Schools, staff roles, students, and caregiver links.
- Fee plans: period, installment amount, due date, status.
- Receipts: amount, date, reference, staff-entered confirmation.
- Reminder preferences and statement exports.

## Main flow
School staff set up a plan and link an authorized caregiver. Staff record received payments. The caregiver sees the schedule, receipts, and remaining balance.

## Operations and safeguards
Enforce school and caregiver access boundaries. Keep an audit trail for balance changes, make corrections reviewable, and avoid storing bank or mobile money credentials.
