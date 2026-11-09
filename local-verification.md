# Local verification walkthrough

Follow the root `README.md` to install and start Claurix, then use this checklist.

## 1. Check the services

- [ ] PostgreSQL and the API are running (Docker mode), or local PostgreSQL and Uvicorn are running (native mode).
- [ ] `http://localhost:8000/docs` opens.
- [ ] `http://localhost:3000` opens.
- [ ] The frontend is configured to call the correct backend URL.

## 2. Check authentication

- [ ] Create a local test account through the app or the documented `POST /auth/signup` route.
- [ ] Sign in through the app or the documented `POST /auth/login` route.
- [ ] Check `GET /auth/me` using the authentication method shown in Swagger.

Use synthetic test details. Obtain the exact request body and response fields from the live Swagger schema; this checklist intentionally does not guess API payload formats.

## 3. Check demo requests

- [ ] Open the demo-request workflow in the frontend.
- [ ] Submit a test request using dummy information.
- [ ] If you have the required internal permissions, inspect `GET /demo-requests`.

The listing route is marked internal in the project documentation. Do not assume ordinary users should have access to it.

## 4. Prepare review material

Before adding screenshots or API examples to this folder:

- Use synthetic names and data.
- Remove email addresses, tokens, OAuth credentials, and identifiers tied to real users.
- Do not include real client or insurer documents.
- Confirm screenshots show the current application version.
