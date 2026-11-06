# EduTranslate AI Architecture

## Overview

EduTranslate is an AI pipeline wrapped in a simple content app. The architectural challenge is choosing a model that handles low resource African languages well, caching results to control cost and making the audio generation smooth on slow networks.

## Suggested stack

- Front end. Next.js PWA.
- API. Node with Nest or Django.
- Database. PostgreSQL.
- Translation. Claude or GPT API for high resource pairs, plus Masakhane community models or Lelapa for low resource pairs where quality is better.
- OCR. Tesseract for simple scans, Claude vision or Gemini vision for messy scans.
- TTS. ElevenLabs where the language is supported, Azure Speech for Swahili and Zulu, or community African TTS models.
- Storage. S3 or Cloudinary for audio and images.

## High level components

1. Web PWA. Input, translate, listen, share.
2. API service.
3. Translation worker. Selects the right model per language pair.
4. OCR worker. Extracts text from images.
5. TTS worker. Generates audio.
6. Review queue for native speakers.

## Data model

- user. id, phone or email, role, languages.
- lesson. id, user_id, title, source_text, source_lang, target_lang, grade_level, status.
- translation. id, lesson_id, model, body, confidence, created_at, reviewed_by.
- audio. id, lesson_id, language, url, duration_seconds.
- share_link. id, lesson_id, token, expires_at.
- correction. id, translation_id, author_id, before, after, created_at.

## API surface

- POST /auth/otp
- POST /lessons, POST /lessons/:id/translate
- POST /lessons/:id/audio
- POST /corrections
- GET /share/:token

## AI pipeline

A translation request picks a backend based on source and target language. For a common pair like English to Swahili, call Claude or GPT with a system prompt that includes idiom notes and the target grade level. For less common pairs, prefer a Masakhane or Lelapa endpoint if available. Cache every translation keyed by hash of source plus target plus grade.

## Deployment

Next.js on Vercel. API and workers on Render or Fly.io. Managed PostgreSQL. Cloudinary for media. AI API keys in a secret store with a monthly spend alarm.

## Scaling considerations

AI cost is the main risk. Keep aggressive caching. Batch audio generation jobs during off peak hours. Push users to short lessons under 200 words in the MVP.
