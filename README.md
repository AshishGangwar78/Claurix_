# Claurix

**AI-native commercial insurance workflow platform** with an enterprise QCR workspace, authentication, and broker placement workflows.

This repository contains the Claurix frontend, backend API, authentication layer, and local development setup.

> **Important:** Do not commit credentials, access tokens, real client RFQs, insurer quotations, or other confidential information.

## Technology stack

| Layer | Technology |
|---|---|
| Frontend | Next.js 14, React, TypeScript, Tailwind CSS |
| Backend | FastAPI, SQLAlchemy |
| Database | PostgreSQL |
| Authentication | JWT (cookie and Bearer token), Google OAuth |
| Infrastructure | Docker Compose (PostgreSQL + API) |

## Prerequisites

- Git
- Node.js 18+
- Python 3.11+
- Docker Desktop with Docker Compose (recommended), or a local PostgreSQL installation

Check your installations:

```bash
git --version
node --version
npm --version
python --version
docker --version
docker compose version
```

## Run Claurix locally

The following steps are for someone setting up the project on a new laptop.

### 1. Clone the repository

Copy the repository's HTTPS or SSH clone URL from GitHub. If the repository is private, the person cloning it must have access.

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd <REPOSITORY_FOLDER>
```

### 2. Create environment files

**macOS / Linux**

```bash
cp .env.example .env
cp backend/.env.example backend/.env
```

**Windows PowerShell**

```powershell
Copy-Item .env.example .env
Copy-Item backend/.env.example backend/.env
```

Edit the generated files:

- Set a strong, unique local `SECRET_KEY`.
- Set `DATABASE_URL` in `backend/.env` to the PostgreSQL database used by the backend.
- Add Google OAuth credentials only if you want to test Google sign-in.
- If required, set `NEXT_PUBLIC_API_URL=http://localhost:8000` in the root `.env.local` file.

Never commit `.env`, `backend/.env`, or `.env.local`. Do not use production credentials for local development.

### 3A. Option A — Docker setup (recommended)

From the repository root:

```bash
docker compose up --build
```

This starts the services configured in `docker-compose.yml` (PostgreSQL and the API).

- API: http://localhost:8000
- Interactive API documentation: http://localhost:8000/docs

To run in the background:

```bash
docker compose up --build -d
```

To view logs and running services:

```bash
docker compose ps
docker compose logs -f
```

To stop:

```bash
docker compose down
```

Avoid `docker compose down -v` unless you intentionally want to remove the database volume and its local data.

### 3B. Option B — Native backend setup (without Docker)

Use this option if PostgreSQL is installed and running on your machine.

```bash
cd backend
python -m venv .venv
```

Activate the environment.

**Windows PowerShell**

```powershell
.venv\Scripts\Activate.ps1
```

**Windows Command Prompt**

```bat
.venv\Scripts\activate.bat
```

**macOS / Linux**

```bash
source .venv/bin/activate
```

Install dependencies and start the API:

```bash
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000
```

Before starting Uvicorn, ensure PostgreSQL is running and `DATABASE_URL` is correctly configured in `backend/.env`.

API: http://localhost:8000  
Docs: http://localhost:8000/docs

### 4. Start the frontend

Open a **new terminal at the repository root** (not inside `backend/`).

```bash
npm install
npm run dev
```

If the repository has a `package-lock.json`, you can use `npm ci` instead of `npm install` for a lockfile-based installation.

Open the app:

- Frontend: http://localhost:3000

If the frontend cannot reach the backend, create or update `.env.local` in the repository root:

```dotenv
NEXT_PUBLIC_API_URL=http://localhost:8000
```

Restart the frontend after changing environment variables.

## Verify your local setup

1. Confirm the backend is running at `http://localhost:8000`.
2. Open `http://localhost:8000/docs` and inspect the available API routes.
3. Confirm the frontend loads at `http://localhost:3000`.
4. Try email signup and login using a synthetic test account.
5. If testing Google sign-in, configure valid OAuth credentials and the local redirect URI.
6. Use Swagger to inspect the exact request and response schemas before sending API requests.

A short checklist is available in [`examples/local-verification.md`](examples/local-verification.md).

## Main API endpoints

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/auth/signup` | Email-based account creation |
| `POST` | `/auth/login` | Email-based login |
| `GET` | `/auth/google` | Start Google OAuth |
| `GET` | `/auth/me` | Get the currently authenticated user |
| `POST` | `/demo-requests` | Submit a demo request |
| `GET` | `/demo-requests` | List demo requests (internal endpoint) |

For exact payload formats, response models, and authentication requirements, refer to the running Swagger documentation at `http://localhost:8000/docs`. Do not assume request schemas from endpoint names alone.

## Project structure

```text
claurix/
├── app/                    # Next.js pages (workspace, auth, dashboard, request-demo)
├── components/             # Reusable React UI
├── lib/                    # Frontend API client
├── backend/
│   ├── app/                # FastAPI application (auth, demo requests, models, config)
│   └── scripts/            # Database initialisation scripts
├── examples/               # Safe examples and local verification notes
├── docker-compose.yml      # PostgreSQL + API services
├── .env.example            # Root environment template
└── README.md
```

This is an overview of the main folders; additional files may exist.

## Troubleshooting

### Frontend loads but API calls fail

- Check that the backend is running.
- Confirm `NEXT_PUBLIC_API_URL` points to the backend.
- Restart the frontend after changing `.env.local`.
- Inspect browser and backend logs.

### Backend cannot connect to PostgreSQL

- Confirm PostgreSQL is running.
- Check database name, username, password, host, and port in `DATABASE_URL`.
- In Docker mode, inspect `docker compose ps` and `docker compose logs`.

### A port is already in use

Check whether another process is using port `3000` or `8000`. Stop that process or configure the relevant service and frontend API URL to use an available port.

### Google sign-in fails

Google OAuth requires valid client credentials and a redirect URI registered for local development. Email signup/login can be used for local testing if Google OAuth has not been configured.

### PowerShell blocks virtual-environment activation

Use the Command Prompt activation command shown above. Avoid changing system-wide security settings solely to activate this project's virtual environment.

## About the `examples/` folder

The `examples/` directory is intended to make the repository easier to understand and review. As you add material, prefer:

- Screenshots of the application populated with synthetic data.
- Sanitised API request/response examples copied from the live Swagger schema.
- Small synthetic demo data, if supported by the application.
- Short walkthroughs of signup, login, workspace navigation, and demo-request submission.

Only include material that is safe to publish. Do not put real client or insurer documents, names, email addresses, policy numbers, tokens, passwords, or OAuth secrets in this folder.

## Security notes

- Keep `.env`, `backend/.env`, and `.env.local` out of Git.
- Never publish OAuth secrets, JWTs, or production credentials.
- Use synthetic or anonymised data in screenshots and examples.
- The `GET /demo-requests` route is documented as internal; do not expose it publicly without the intended access controls.

## License

Private / all rights reserved unless otherwise specified by the repository owner.
