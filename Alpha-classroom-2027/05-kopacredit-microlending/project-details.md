# KopaCredit Microlending

A micro loan origination and servicing platform for microfinance institutions and SACCOs.

## Problem

Microfinance institutions, SACCOs and informal lenders in Africa still process small loans with paper forms, Excel sheets and WhatsApp messages. The result is slow decisioning, poor repayment tracking and high default rates. At the same time, millions of small traders and gig workers have no access to short term credit for stock or emergencies.

KopaCredit gives a lender a lightweight origination, approval and repayment workflow and gives the borrower a simple application and visibility into their balance.

## Target users

Primary users are small to mid size microfinance institutions, SACCOs and informal lending associations. Secondary users are the borrowers applying for loans of 20 to 500 US dollars.

## MVP features

1. Borrower profile. Phone, ID, address, income statement declared by borrower.
2. Scoring. A simple score from declared income, number of dependents and optional import of a ShopBook sales statement.
3. Loan request. Amount, purpose, repayment term.
4. Approval workflow. Loan officer review, committee sign off, approval or decline with reason.
5. Repayment schedule. Weekly or monthly schedule with amount due.
6. Repayment capture. Officer logs a repayment with mobile money reference.
7. Portfolio dashboard. Total outstanding, due today, overdue, portfolio at risk.

## Post MVP ideas

M-Pesa or Flutterwave direct repayment pull, borrower self service portal with reminders, machine learning scoring with repayment history, credit bureau reporting, insurance bolt on, group lending.

## User stories

As a loan officer I want to approve a 150 dollar loan in under 10 minutes so that my institution can serve more clients a day. As a borrower I want to see what I still owe so that I never miss a due date. As the branch manager I want to see portfolio at risk at a glance.

## Success metrics

Loans originated per week, approval cycle time, portfolio at risk, repayment rate, retention of partner institutions.

## Revenue model

SaaS fee per institution with price tiered by active portfolio size. Per loan processing fee. Setup fee for migration.

## Risks and compliance

Lending is regulated in most African countries. The MVP serves licensed institutions. The platform must not advertise itself as a direct lender. Protect KYC data at rest and in transit. Provide an export for regulator reporting where the country requires it.

## Research references

CGAP microfinance reports, Center for Financial Inclusion, Kenya SACCO regulatory authority publications, case studies on Musoni, Tala, Branch and Jumo.
