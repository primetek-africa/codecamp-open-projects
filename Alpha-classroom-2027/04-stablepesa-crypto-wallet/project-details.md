# StablePesa Crypto Wallet

A phone first custodial wallet that lets African users hold and send US dollar stablecoins for savings and remittance.

## Problem

Several African currencies have lost 20 to 60 percent of their value against the dollar in the last five years. At the same time, cross border bank transfers remain slow and expensive. Stablecoins like USDC and cUSD on networks such as Celo and Base are already used by freelancers and traders, but tools built for developers are not friendly to a first time user who prefers phone numbers over 42 character wallet addresses.

StablePesa gives a user a simple phone number based wallet that can send and receive stablecoins locally and off ramp to mobile money through a listed partner agent.

## Target users

Primary users are young urban earners paid in foreign currency, freelancers working with overseas clients and small cross border traders moving goods between neighboring countries. Secondary users are agents who provide the on and off ramp with local cash and mobile money.

## MVP features

1. Phone number sign up with KYC lite. National ID photo and selfie, stored encrypted.
2. Custodial wallet. One address per user, platform holds the key in a secure vault for the MVP.
3. Send to a phone number. Behind the scenes, the app resolves the number to an address.
4. Receive by sharing a username or QR code.
5. Balance view in USD and in local currency using a daily rate.
6. Off ramp request to a listed agent. Agent confirms receipt of stablecoin in app and pays mobile money out of band. MVP runs on a testnet.
7. Transaction history with references.

## Post MVP ideas

Non custodial upgrade with a seed phrase, in app swap to other stablecoins, yield product with a regulated partner, merchant payment link, tax export.

## User stories

As a freelancer in Lagos I want to receive USDC from my client and hold it until my next rent payment so that my savings do not lose value. As a trader in Ouagadougou I want to send USDC to my supplier in Lome so that I avoid cash and currency loss.

## Success metrics

Verified users, monthly active senders, average transfer size, off ramp success rate, retention after 60 days.

## Revenue model

Spread on off ramp, flat fee on larger transfers, agent subscription.

## Risks and compliance

Crypto rules differ sharply by country. The MVP must run on a testnet and clearly advertise itself as an educational prototype. Do not take real customer funds. Keep KYC minimal and encrypt all identity documents. Never claim to be a bank or a money services business. For a real launch, pursue licensing in the target country or partner with a licensed custodian.

## Research references

GSMA stablecoin reports, Chainalysis Africa adoption index, Celo and Base developer docs, Yellow Card, Mara, Fonbnk, Jambo and Grey case studies.
