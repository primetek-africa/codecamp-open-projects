# ExamPrep AI Architecture

## Overview

ExamPrep AI is a learning product with two architectural pillars. A content pipeline that turns past papers into a reliable question bank, and an AI explanation pipeline that controls cost and quality.

## Suggested stack

- Front end. Next.js PWA. Static rendering for marketing and SEO on subject pages.
- API. Node with NestJS or Django with DRF. Django is friendly for the content pipeline.
- Database. PostgreSQL.
- AI. Claude or GPT API. Cache explanations by question id.
- Search. Postgres full text search for question content.
- Email. Resend or Mailgun for parent summaries.
- Storage. Cloudinary for question images and figures.

## High level components

1. Learner app. Practice, mock exam, progress.
2. Parent view. Weekly summary.
3. Teacher content studio. Review and approve AI explanations, flag bad questions.
4. API service.
5. Content import pipeline. Takes PDFs of past papers, extracts questions, cleans them, stores with metadata.
6. AI explanation worker. Generates explanations on demand with caching.

## Data model

- learner. id, email_or_phone, dob, subjects, country, exam_board.
- parent_link. id, learner_id, parent_contact, token, consent.
- subject. id, name, board, grade.
- topic. id, subject_id, name.
- question. id, topic_id, type, body_md, choices_json, correct_answer, difficulty, source.
- explanation. id, question_id, body_md, model, reviewed_by, reviewed_at.
- session. id, learner_id, subject_id, started_at, ended_at, score.
- attempt. id, session_id, question_id, answer, correct.

## API surface

- POST /auth/otp
- GET /subjects, GET /topics
- POST /sessions, GET /sessions/:id/next-question, POST /attempts
- GET /explanations/:question_id
- POST /mock-exams, GET /parent/summary

## AI pipeline

For each question, the first learner to request an explanation triggers a generation. The result is cached keyed by question id. A teacher can edit and sign off, which marks the explanation reviewed. Reviewed explanations are shown with a tick. Unreviewed ones are shown with an AI generated label. Guardrails include a system prompt that restricts the model to a syllabus, refusal for off topic chat, and a temperature low enough for consistency.

## Content pipeline

Past paper PDFs go through a parser that extracts text and figures. A human curator fixes OCR errors. Questions carry a source tag and page reference so learners can trust the origin.

## Deployment

Next.js on Vercel. API and workers on Render or Fly.io. Managed PostgreSQL. Secrets for AI keys in platform vault. Cost dashboard on AI usage per day.

## Scaling considerations

Explanation generation is the main cost. Cache aggressively. Batch review in the content studio. For mock exams, pre compute the question set to avoid holding a session open while waiting for AI.
