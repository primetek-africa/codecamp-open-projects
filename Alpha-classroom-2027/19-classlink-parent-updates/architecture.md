# ClassLink Parent Updates Architecture

## Shape
A staff dashboard and caregiver-facing message view use one secured API and database. A notification adapter supports manual or mock delivery before adding an SMS provider.

## Main modules and records
- School, class, learner, caregiver link, and staff role.
- Attendance notice and class announcement.
- Delivery attempt, contact preference, and opt-out state.
- Audit record for staff changes.

## Main flow
Authorized staff select a class and prepare a notice. The app resolves opted-in caregivers and records delivery attempts. Caregivers view messages relevant to their linked learners.

## Operations and safeguards
Verify caregiver links and staff access. Limit collected student information and message content. Support opt-out, access logging, and deletion according to school policy.
