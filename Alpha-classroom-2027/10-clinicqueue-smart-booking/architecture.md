# ClinicQueue Architecture

## Overview

ClinicQueue is a small realtime system. The live queue board is the key interaction, so the architecture centers on fast broadcast and a reliable queue state.

## Suggested stack

- Front end. Vue 3 or Nuxt PWA. A staff tablet friendly layout.
- API. Laravel with Reverb for realtime, or Node with Socket.IO.
- Database. MySQL.
- Realtime. Pusher, Laravel Reverb or Socket.IO.
- QR. qrcode library on the server.
- SMS. Africa's Talking or Twilio.

## High level components

1. Patient PWA. Book appointment, see queue status.
2. Staff PWA. Issue token, move queue forward.
3. Public queue board. A read only screen for a TV in the waiting room.
4. API service.
5. Realtime channel service.

## Data model

- clinic. id, name, timezone, opening_hours.
- service. id, clinic_id, name, estimated_minutes.
- staff. id, clinic_id, phone, role.
- slot. id, clinic_id, service_id, starts_at, ends_at, capacity.
- appointment. id, slot_id, patient_name, patient_phone, status.
- queue_token. id, clinic_id, service_id, number, qr_code, patient_phone, status, created_at, called_at.
- visit. id, token_id, started_at, ended_at.

## API surface

- POST /auth/staff/login
- POST /appointments, POST /tokens
- PATCH /tokens/:id/call, PATCH /tokens/:id/done
- GET /clinics/:id/queue (SSE or socket)

## Realtime

The queue state is broadcast on a channel per clinic. When a token is called, the API updates the token status and pushes an event. The public board and the patient PWA subscribe. If sockets are not available for a given patient, fall back to polling every 10 seconds.

## Deployment

Hostinger, Railway, Render or DigitalOcean. Managed MySQL. Pusher is the cheapest realtime path for an MVP. Move to Laravel Reverb or a self hosted Socket.IO cluster when active clinics pass 50.

## Scaling considerations

The hot writes are status transitions. Add an index on queue_token(clinic_id, status, created_at). Keep the board query small by paginating waiting tokens. Record every status change for audit and analytics.
