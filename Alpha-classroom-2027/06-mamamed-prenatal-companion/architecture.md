# MamaMed Architecture

## Overview

MamaMed is a reminder engine with a small clinic dashboard. The design must respect low literacy users, low end phones and intermittent connectivity.

## Suggested stack

- Mother facing app. PWA built with Vue or React. Large buttons, few words, picture icons. Optional React Native wrapper later.
- Clinic dashboard. Nuxt or Next.js.
- API. Laravel or Node. Laravel is a strong fit for a resource heavy CRUD domain with queued reminders.
- Database. MySQL or PostgreSQL. Encrypted columns for identifiers.
- Reminders. Africa's Talking SMS, Twilio WhatsApp Business API.
- Cron. Laravel scheduler or node-cron.

## High level components

1. Mother PWA. Opt in, schedule view, danger sign checker, education cards.
2. Clinic dashboard. Patient register, follow up list, reports.
3. API service. Patients, schedules, messages, content.
4. Reminder worker. Reads due reminders and dispatches via SMS or WhatsApp.
5. Content service. Serves reviewed cards with a source and reviewer attribution.
6. NGO reporting module.

## Data model

- clinic. id, name, country, county, phone.
- mother. id, clinic_id, phone_encrypted, lmp_date or child_dob, language, status.
- visit. id, mother_id, kind (anc1, anc2, postnatal1, immunization_6w), due_on, status.
- reminder. id, mother_id, visit_id, channel, scheduled_for, sent_at, delivered.
- content_card. id, title, body, reviewer, language, tags.
- follow_up_task. id, mother_id, clinic_id, reason, assigned_to, closed_at.

## API surface

- POST /auth/clinic/login
- POST /mothers, GET /mothers/:id
- GET /clinics/:id/follow-ups
- GET /content, GET /content/:id
- POST /mothers/:id/opt-out
- GET /reports/program

## Reminders

A scheduler runs every 15 minutes. It queries reminders where scheduled_for is in the next window and status is pending, then dispatches via the right channel. If delivery fails, retry with a different channel. Store the delivery receipt. Record opt out as a hard stop.

## Privacy

Phone numbers are stored encrypted with a key held in the deployment secret store. Mother names are optional. Danger sign checker results are not shared with anyone unless the mother opts in. Logs scrub personal data.

## Deployment

Low cost managed hosting. Hostinger, Railway, Render or DigitalOcean. Managed MySQL. Store content in the database rather than markdown files to make clinic approval easier. Keep daily backups.

## Scaling considerations

Reminder throughput is the hot path. Partition reminders by day and shard by clinic_id if volume grows. Avoid sending many reminders at the same minute by jittering schedules across a 5 minute window.
