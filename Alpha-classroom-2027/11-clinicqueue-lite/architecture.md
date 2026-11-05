# ClinicQueue Lite Architecture

## Shape
A patient-facing responsive client and a staff dashboard share one API and database. A single application with patient, clinic, and administration modules is appropriate for the MVP.

## Main modules and records
- Clinic, staff accounts, services, and availability slots.
- Appointment requests with minimal patient contact details.
- Queue entries with token, status, and clinic reference.
- Consent and notification preferences.

## Main flow
A patient requests an available slot. Staff confirm it and add the patient to the daily queue. Staff update visit status; the patient sees only their own appointment and queue information.

## Operations and safeguards
Enforce staff roles and patient record access. Use secure authentication, audit administrative access, define retention and deletion, and do not store diagnoses or clinical notes in the MVP.
