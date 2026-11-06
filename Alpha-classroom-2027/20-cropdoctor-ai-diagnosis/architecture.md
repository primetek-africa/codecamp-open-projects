# CropDoctor Architecture

## Overview

CropDoctor is an inference product. The architecture must deal with poor connectivity, heavy image payloads and the realities of running an ML model at low cost.

## Suggested stack

- Mobile. React Native with Expo or Flutter. Camera first layout.
- API. Django or Node.
- Database. PostgreSQL.
- Model. Fine tuned image model hosted on Hugging Face Inference or Replicate. Alternatively, a Keras or PyTorch model served from a GPU instance.
- OCR and vision support. Optional Claude vision for a fallback explanation step.
- Storage. S3 compatible for photos.

## High level components

1. Farmer mobile app. Capture, diagnose, history.
2. Extension web app. Chat, outbreak map.
3. API service.
4. Inference service that wraps the model endpoint.
5. Treatment content service with reviewed cards.

## Data model

- farmer. id, phone, county, crops_json.
- plot. id, farmer_id, name, lat, lng, size_acres.
- diagnosis. id, plot_id, image_url, top1_label, top1_confidence, model_version, created_at.
- diagnosis_label. id, diagnosis_id, label, confidence, rank.
- treatment_card. id, label, crop, body_md, language, reviewer, reviewed_at.
- expert_chat. id, farmer_id, diagnosis_id, assigned_to, status.

## API surface

- POST /auth/otp
- POST /diagnoses with a multipart image
- GET /diagnoses/:id
- GET /treatment-cards/:label?lang=sw
- POST /expert-chats

## Inference

The server receives an image, resizes it to the model input size, sends it to the inference endpoint and stores the top results. If confidence is below a threshold, mark the diagnosis uncertain and nudge the farmer to request an expert chat.

## Offline

The mobile app stores photos locally and queues diagnosis calls. When online, the app uploads the photos and receives results in order. UI shows pending diagnoses with a clock icon.

## Deployment

Mobile via Expo EAS. API on Render or Fly.io. Inference on Hugging Face or Replicate, with a GPU backup on Modal or Banana for peak load. Managed PostgreSQL. S3 for photos.

## Scaling considerations

Inference is the main cost. Cache by image hash to avoid repeat cost from the same photo. Compress images aggressively on device before upload. Batch inference requests where possible.
