# StablePesa Architecture

## Overview

StablePesa is a custodial wallet with a simple phone number directory on top of an EVM compatible chain. Custody simplifies the UX and reduces the risk of a user losing a seed phrase, but it raises the bar on security of the key vault.

## Suggested stack

- Mobile. React Native with Expo, or Flutter.
- Web dashboard. Next.js for support and agent tools.
- API. Node with NestJS or Django. NestJS is attractive because the TypeScript types carry well across to the mobile client.
- Database. PostgreSQL for user data. MongoDB is fine if the team prefers it.
- Blockchain client. viem or ethers.
- Chain. Celo Alfajores testnet or Base Sepolia for the MVP. Mainnet only after licensing.
- Key vault. AWS KMS, GCP KMS or HashiCorp Vault. Never store private keys in the app database.
- SMS and WhatsApp. Africa's Talking, Twilio.

## High level components

1. Mobile app. Onboarding, wallet, send, receive, off ramp.
2. API service. Users, KYC, directory, transactions, off ramp requests.
3. Custody service. Isolated service that signs transactions by calling KMS. The main API never sees the raw key.
4. Blockchain watcher. Indexes incoming transactions and credits the user balance view.
5. Agent app. Agent accepts off ramp requests and confirms mobile money payouts.
6. Admin panel. Compliance review, user support.

## Data model

- user. id, phone, username, country, kyc_status, created_at.
- kyc_document. id, user_id, type, image_url_encrypted, reviewed_by.
- wallet. id, user_id, chain, address, key_ref (pointer to KMS key id).
- transaction. id, wallet_id, chain_tx_hash, direction, amount, token, counterparty, status, created_at.
- offramp_request. id, user_id, amount_token, amount_local, agent_id, status, mobile_money_ref.
- agent. id, name, city, supported_methods_json, stake_amount.

## API surface

- POST /auth/otp and /auth/verify
- POST /kyc/document
- GET /wallet, GET /wallet/balance
- POST /send with { recipient_username_or_phone, amount }
- POST /offramp/requests, POST /offramp/requests/:id/confirm
- GET /transactions

## Security

Private keys live only in KMS. The custody service exposes a sign RPC that checks a one time password or passkey challenge before signing. The API database stores only key references. Add a withdrawal whitelist and a 24 hour daily limit in code. Log every signing event with tamper evident storage.

## Deployment

Mobile builds via Expo EAS. API and workers on Fly.io, Render or AWS. KMS in the same cloud as the custody service. Managed PostgreSQL. Use a bastion for database access.

## Scaling considerations

Blockchain watcher must be idempotent because it will see the same tx multiple times during reorgs. Use a single writer per shard for signing to avoid nonce races. Cache chain reads behind a short Redis cache.
