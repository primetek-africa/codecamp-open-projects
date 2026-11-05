# LocalTutor Match Architecture

## Shape
A public tutor search and private booking area use one API and database. Add a moderation dashboard for profiles and reports.

## Main modules and records
- Tutor profiles, subjects, languages, and service areas.
- Availability and lesson requests.
- Booking status, learner feedback, and report queue.
- Optional subscription or booking-fee records.

## Main flow
A learner filters tutors and requests a lesson. The tutor accepts or declines. After the lesson, the learner can leave feedback that is checked against a completed booking.

## Operations and safeguards
Moderate profiles and reviews, minimize public personal information, and add reporting and blocking. Limit access to caregiver and learner data and make tutor verification level visible.
