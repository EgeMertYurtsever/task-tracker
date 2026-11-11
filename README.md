# Task Tracker

A small task manager web app built with Flask and SQLite, with planned TOTP two-factor login. Work in progress — built as a learning project.

## Why this project

I want to understand how web authentication works from the inside, not only how to use it. The goal is a complete, small app where users register, log in with a password and a one-time code from an authenticator app, and manage their own tasks. I plan to implement the TOTP algorithm (RFC 6238) myself and verify it against an established library.

## Features

- [x] Flask app running locally
- [x] Database schema for users
- [ ] User registration and login (passwords stored as hashes)
- [ ] Create, list, update and delete tasks (each user sees only their own)
- [ ] TOTP two-factor login (Google Authenticator), with my own TOTP implementation tested against `pyotp`
- [ ] Unit tests
- [ ] Dockerfile
- [ ] GitHub Actions running the tests

## Tech stack

- Python 3.13
- Flask
- SQLite (Python's built-in `sqlite3` module)

## Project structure

```
task-tracker/
├── app.py            # Flask application and routes
├── db.py             # database connection helper (in progress)
├── schema.sql        # database tables
├── requirements.txt  # pinned dependencies (pip freeze)
├── .gitignore        # keeps the virtual environment, database and secrets out of Git
└── README.md
```

## How to run (Windows PowerShell)

```powershell
git clone https://github.com/EgeMertYurtsever/task-tracker.git
cd task-tracker
py -3.13 -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
flask --app app run --debug
```

Then open http://127.0.0.1:5000

> `--debug` is for local development only. It enables an interactive debugger and must never be used in production.

## Design decisions

- **Virtual environment + pinned `requirements.txt`**: each project has isolated dependencies, and anyone can reproduce the exact same setup.
- **Plain `sqlite3` instead of an ORM (SQLAlchemy)**: I want to learn and write real SQL, with less hidden behaviour to explain. The trade-off is that I must use `?` placeholders in every query myself to prevent SQL injection.
- **One database connection per request**: the connection is stored in Flask's `g` object, reused within a request and closed when the request ends.
- **Secrets and the database are never committed**: `.gitignore` excludes `.env` and `*.db`, because the database will contain password hashes.
- **Usernames are unique at the database level** (`UNIQUE` constraint), so duplicates are rejected even if application code has a bug.