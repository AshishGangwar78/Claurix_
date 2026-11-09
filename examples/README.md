# Claurix examples

This directory contains safe, non-production material to help reviewers and developers understand and verify a local Claurix setup.

## Suggested contents

As the project is prepared for review, useful additions include:

1. **UI screenshots** captured with synthetic data.
2. **API examples** copied from the running Swagger documentation at `http://localhost:8000/docs`.
3. **Synthetic demo data**, if supported by the application.
4. **Short walkthroughs** showing signup, login, workspace navigation, and demo-request submission.

## Current contents

- `local-verification.md` — checklist for checking the local frontend and backend.

## Keep this folder safe

Do not add `.env` files, OAuth secrets, JWTs, real client RFQs, insurer quotations, or personal information. Inspect screenshots and sample payloads for names, email addresses, policy numbers, tokens, and other confidential fields before committing them.
