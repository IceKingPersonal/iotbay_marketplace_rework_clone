# IoTBay Marketplace Rework (R0 + Feature 01)

This repository contains:
- `backend_rework`: Flask + SQLite backend for user registration, login, profile, and access logging.
- `frontend_rework`: React + Vite frontend for the same flows.

This guide covers the correct setup order, dependency installation, backend/frontend startup, and basic validation using the seeded accounts from `backend_rework/init_db.py`.

## 1) Prerequisites

- Python 3.11+ (3.12 tested)
- Node.js 20+
- npm 10+

## 2) Backend setup (initialize first)

Run from repository root:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

### Why `requirements.txt`

Python dependencies used directly by backend code are now centralized in `requirements.txt` so setup is a single command.

## 3) Initialize database and seed test users

From repository root (with virtualenv activated):

```bash
python backend_rework/init_db.py
```

This creates `iotbay.db` and inserts initial users (if not already present):

- Customer: `customer@test.com` / `Password123`
- Staff: `staff@test.com` / `Password123`

## 4) Start backend API

From repository root (with virtualenv activated):

```bash
python backend_rework/app.py
```

Backend runs on `http://127.0.0.1:5000`.

## 5) Frontend setup and start

Open a second terminal in repository root:

```bash
cd frontend_rework
npm install
npm run dev
```

Frontend runs on `http://localhost:5173` and is already allowed by backend CORS settings.

## 6) Correct initialization sequence

1. Create + activate Python virtualenv.
2. Install Python packages with `pip install -r requirements.txt`.
3. Initialize database with `python backend_rework/init_db.py`.
4. Start backend (`python backend_rework/app.py`).
5. Install frontend packages (`npm install`).
6. Start frontend (`npm run dev`).
7. Log in on frontend using seeded test accounts.

## 7) Basic test flow (Feature 01)

1. Open frontend at `http://localhost:5173`.
2. Log in as customer and verify dashboard/profile access.
3. Log out and log in as staff.
4. Verify access logs page loads and contains entries.

## 8) Notes from code inspection

- Backend depends on `Flask`, `Flask-Cors`, `python-dotenv`, and `Werkzeug`.
- `backend_rework/config.py` loads dotenv but currently uses hardcoded `SECRET_KEY` and `DATABASE` values.
- Backend uses relative module imports expecting execution from repository root as shown above.

## 9) Useful commands

Backend checks:

```bash
python -m py_compile backend_rework/app.py backend_rework/config.py backend_rework/database.py backend_rework/init_db.py backend_rework/routes/*.py backend_rework/models/*.py backend_rework/services/*.py backend_rework/utils/*.py
```

Frontend checks:

```bash
cd frontend_rework
npm run build
npm run lint
```
