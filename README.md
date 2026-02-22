# Khooni Backend

Backend API for a blood donation / emergency assistance platform. Built with **FastAPI** + **SQLAlchemy** and backed by **PostgreSQL**.

---

## What this project does

Khooni Backend provides a REST API to:

- Register and authenticate users
- Store user profiles (donors / receivers)
- Store emergency contacts linked to users
- Persist data in PostgreSQL using SQLAlchemy ORM models

Primary entrypoint: `main.py`.

---

## Why this project is useful

- **FastAPI** gives a modern Python web API with automatic OpenAPI docs.
- **PostgreSQL + SQLAlchemy** provides a robust relational persistence layer.
- **Pydantic schemas** validate and document request/response payloads.
- **Docker Compose** is included for quickly starting a local PostgreSQL instance.

---

## Tech stack

- **Python** (FastAPI)
- **PostgreSQL**
- **SQLAlchemy 2.x**
- **Pydantic 2.x**
- **Uvicorn** (ASGI server)
- **passlib + bcrypt** (password hashing)

Dependencies are listed in `requirements.txt`.

---

## Project structure

- `main.py` — FastAPI app and API routes
- `database.py` — SQLAlchemy engine/session setup (currently uses a hardcoded DB URL)
- `models.py` — SQLAlchemy ORM models (User, Donor, Receiver, EmergencyContact + enums)
- `schema.py` — Pydantic schemas for request/response validation
- `compose.yml` — Docker Compose for PostgreSQL
- `requirements.txt` — Python dependencies

---

## Getting started

### Prerequisites

- Python 3.10+ recommended
- Docker (optional, for running PostgreSQL via compose)
- A PostgreSQL instance (local or container)

### 1) Start PostgreSQL (Docker Compose)

This repo includes `compose.yml`:

```bash
docker compose up -d
```

By default, the compose file starts PostgreSQL on:

- Host: `localhost`
- Port: `5432`
- DB: `khooni`
- User: `pakshi`
- Password: `12345678`

### 2) Create and activate a virtual environment

```bash
python -m venv .venv
# macOS/Linux
source .venv/bin/activate
# Windows (PowerShell)
.venv\Scripts\Activate.ps1
```

### 3) Install dependencies

```bash
pip install -r requirements.txt
```

### 4) Run the API

```bash
uvicorn main:app --reload
```

The API will be available at:

- http://127.0.0.1:8000

Interactive API docs:

- Swagger UI: http://127.0.0.1:8000/docs
- ReDoc: http://127.0.0.1:8000/redoc

---

## Configuration

### Database connection

The DB connection string is currently hardcoded in `database.py`:

- `postgresql://pakshi:12345678@localhost:5432/khooni`

If you need a different database configuration, you’ll need to update `DATABASE_URL` in `database.py` (a future improvement would be to read this from environment variables).

---

## Usage examples

### Check whether a user exists

`GET /user_exists?email=someone@example.com`

Example:

```bash
curl "http://127.0.0.1:8000/user_exists?email=someone@example.com"
```

### Register

`POST /register` (JSON)

```bash
curl -X POST "http://127.0.0.1:8000/register" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Jane Doe",
    "email": "jane@example.com",
    "password": "supersecret123",
    "userType": "donor"
  }'
```

### Login

`POST /login` (JSON)

```bash
curl -X POST "http://127.0.0.1:8000/login" \
  -H "Content-Type: application/json" \
  -d '{
    "email": "jane@example.com",
    "password": "supersecret123"
  }'
```

For the full and most accurate list of available endpoints and schemas, use the OpenAPI docs at `/docs`.

---

## Where users can get help

- Use the built-in API documentation:
  - http://127.0.0.1:8000/docs
- If you encounter a bug or want a feature, please open a GitHub Issue in this repository:
  - `https://github.com/GuessWho121/Khooni-Backend/issues`

---

## Contributing

Contributions are welcome.

Suggested workflow:

1. Fork the repo
2. Create a feature branch
3. Make changes with clear commits
4. Open a Pull Request

If you add new endpoints:
- include/update Pydantic schemas in `schema.py`
- add/adjust SQLAlchemy models in `models.py` (if needed)
- keep endpoints documented via FastAPI docstrings and response models where possible

---

## Maintainers

- Maintained by **@GuessWho121**

---
