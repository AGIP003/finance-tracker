# Personal Finance Tracker

A full-stack personal finance tracker with JWT authentication, PostgreSQL persistence, transaction management, dashboard charts, API documentation, and Telegram bot transaction capture.

Live frontend: https://moneytiqx.vercel.app  
API docs: https://finance-tracker03.up.railway.app/api/docs

## Overview

Personal Finance Tracker helps users record income and expenses, review spending patterns, and add transactions from either the web app or Telegram. The Telegram bot supports lightweight natural-language entry for common money events, while the web dashboard provides transaction history, summaries, charts, and account management flows.

This project is also a record of production-minded backend engineering practice: authentication, validation, migrations, environment-based configuration, request logging, rate limiting, CORS, deployment configuration, and frontend test coverage.

## Core Features

- User registration, login, JWT sessions, and protected routes
- Password reset flow with email delivery support
- Transaction CRUD with ownership checks so users only access their own records
- Transaction search, filtering, dashboard summaries, and charts
- PostgreSQL-backed data model with SQLAlchemy ORM classes and Alembic migrations
- Telegram account linking with expiring link tokens
- Telegram bot commands for linking, adding transactions, balances, category aliases, and default payment methods
- Flask-RESTX API documentation at `/api/docs`
- Frontend error boundaries, protected routing, responsive layouts, and test coverage
- Deployment-ready backend process via Gunicorn and frontend config for Vercel

## Tech Stack

Backend:
- Python
- Flask
- PostgreSQL
- SQLAlchemy
- Alembic / Flask-Migrate
- Flask-RESTX
- JWT
- Flask-Bcrypt
- Flask-Mail
- Flask-Limiter
- Gunicorn

Frontend:
- React
- Vite
- React Router
- Axios
- Recharts
- React Hook Form
- React Hot Toast
- Lucide React
- Vitest
- Testing Library

Bot and infrastructure:
- python-telegram-bot
- Railway for the backend API
- Vercel for the frontend

## Architecture

```text
React frontend (Vercel)
        |
        v
Flask API (Railway) ---- PostgreSQL
        ^
        |
Telegram bot worker
```

The frontend calls the Flask API through `VITE_API_URL`. The API validates JWTs, scopes records by user, and persists data in PostgreSQL. The Telegram bot authenticates to the API and can create transactions after a user links their Telegram account.

## Project Structure


app/                  Flask application, routes, auth, docs, middleware
app/models/           SQLAlchemy model definitions
bot/                  Telegram bot handlers and parser logic
config/               Environment-based application configuration
finance_tracker/      Earlier CLI/data utilities retained for project history
migrations/           Alembic migration setup
scripts/              Data migration/helper scripts
tracker-frontend/     React/Vite frontend


## Local Setup

### Backend

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
```

Fill in `.env` with local database credentials, a `SECRET_KEY`, frontend URL, and any optional mail or Telegram settings.

Run the Flask API:

```bash
flask --app run run
```

The API exposes a health check at:

```text
GET /health
```

API documentation is available at:

```text
/api/docs
```

### Database Migrations

After configuring the database:

```bash
flask --app run db upgrade
```

### Frontend

```bash
cd tracker-frontend
nvm use
npm install
npm run dev
```

The frontend expects:

```text
VITE_API_URL=http://localhost:5000/api
```

### Telegram Bot

From the project root:

```bash
python -m bot.main
```

Required environment variables include:

```text
TELEGRAM_BOT_TOKEN
API_BASE_URL
```

## Quality Checks

Frontend:

```bash
cd tracker-frontend
nvm use
npm run lint
npm test -- --run
npm run build
```

Current frontend baseline:

```text
lint: passing
tests: 10 passing
build: passing
```

Backend smoke checks:

```bash
python -m compileall app bot config finance_tracker
flask --app run routes
```

## Deployment Notes

- Frontend is configured for Vercel.
- Backend runs with Gunicorn using the project `Procfile`.
- Production API configuration is environment-driven through `.env` values.
- Keep real `.env` files out of git. Use `.env.example` as the public template.

## Roadmap

- Reconnect deployments to this standalone repository after the public repo is stable
- Add screenshots and demo walkthroughs
- Expand backend tests around auth, transaction ownership, and Telegram linking
- Add CI for lint, test, and build checks
- Continue converting mock finance modules into persisted backend-backed features

## Author

Juma Wangai  
GitHub: https://github.com/AGIP003

Built as part of a structured backend engineering program, documenting real production decisions and trade-offs while growing the project into a deployed full-stack application.
