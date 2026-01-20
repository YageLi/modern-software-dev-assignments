# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a course assignments repository for CS146S (The Modern Software Developer). Each `week*/` folder contains a self-contained assignment. The main development focus is **week4/**, which contains a FastAPI + SQLite starter application ("developer's command center").

## Environment Setup

- Python 3.12 with Conda: `conda activate cs146s`
- Dependencies managed by Poetry: `poetry install --no-interaction`

## Week 4 Starter App Commands

All commands run from `week4/` directory:

```bash
make run          # Start dev server at localhost:8000
make test         # Run pytest
make format       # Run black + ruff --fix
make lint         # Run ruff check
make seed         # Re-seed database
```

Run a single test file:
```bash
PYTHONPATH=. pytest -q backend/tests/test_notes.py
```

Run a single test:
```bash
PYTHONPATH=. pytest -q backend/tests/test_notes.py::test_create_note -v
```

## Week 4 Architecture

**Backend (FastAPI)**
- Entry point: `backend/app/main.py` - mounts routers, static files, runs DB setup on startup
- Database: `backend/app/db.py` - SQLite via SQLAlchemy, uses `get_db()` dependency for sessions
- Models: `backend/app/models.py` - `Note` and `ActionItem` tables
- Schemas: `backend/app/schemas.py` - Pydantic models for request/response validation
- Routers: `backend/app/routers/` - `notes.py` and `action_items.py` define API endpoints
- Services: `backend/app/services/extract.py` - text parsing utility

**Frontend**
- Static files in `frontend/` served by FastAPI at `/static`
- `index.html` loads `app.js` which calls the backend API

**Data**
- SQLite DB at `data/app.db` (created on first run)
- Seed data in `data/seed.sql` (applied automatically if DB is new)

**Tests**
- Located in `backend/tests/`
- Use `conftest.py` fixture that creates an isolated temp SQLite DB per test

## Code Style

- Formatter: black (line-length 100)
- Linter: ruff (rules: E, F, I, UP, B; ignores E501, B008)
- Pre-commit hooks available: `pre-commit install`
