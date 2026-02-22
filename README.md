# Khooni Backend

A RESTful API backend for **Khooni**, a blood donation platform that connects blood donors with recipients. Built with FastAPI and PostgreSQL, it handles user registration, donor/recipient profiles, and emergency contact management.

## Features

- **User authentication** – Register and log in with bcrypt-hashed passwords
- **Donor profiles** – Store blood type, date of birth, gender, and contact information
- **Recipient profiles** – Store required blood type and contact information
- **Emergency contacts** – Attach up to two emergency contacts per user
- **User dashboard** – Retrieve a complete snapshot of any user's profile
- **CORS-enabled** – Ready to be consumed by any frontend origin

## Tech Stack

| Layer | Technology |
|-------|-----------|
| API framework | [FastAPI](https://fastapi.tiangolo.com/) 0.115 |
| ORM | [SQLAlchemy](https://www.sqlalchemy.org/) 2.0 |
| Database | PostgreSQL 16+ |
| Validation | Pydantic 2 |
| Password hashing | passlib (bcrypt) |
| ASGI server | Uvicorn |

## Prerequisites

- Python 3.10+
- [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/) (for the database)

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/GuessWho121/Khooni-Backend.git
cd Khooni-Backend
```

### 2. Start the PostgreSQL database

```bash
docker compose up -d
```

This starts a PostgreSQL instance with the credentials defined in `compose.yml`:

| Setting | Value |
|---------|-------|
| Host | `localhost` |
| Port | `5432` |
| Database | `khooni` |
| User | `pakshi` |
| Password | `12345678` |

### 3. Install Python dependencies

```bash
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### 4. Run the API server

```bash
uvicorn main:app --reload
```

The API is now available at `http://localhost:8000`.  
Interactive docs (Swagger UI) are at `http://localhost:8000/docs`.

## API Reference

### Authentication

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/register` | Create a new user account |
| `POST` | `/login` | Authenticate and retrieve user info |
| `GET` | `/user_exists?email=` | Check whether an email is already registered |
| `GET` | `/user_by_email?email=` | Look up a user by email address |

**Register a new user**

```bash
curl -X POST http://localhost:8000/register \
  -H "Content-Type: application/json" \
  -d '{"name": "Ali Khan", "email": "ali@example.com", "password": "secret123", "userType": "donor"}'
```

**Log in**

```bash
curl -X POST http://localhost:8000/login \
  -H "Content-Type: application/json" \
  -d '{"email": "ali@example.com", "password": "secret123"}'
```

### Profiles

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/submit-donor-form` | Create a donor profile for an existing user |
| `POST` | `/submit-recipient-form` | Create a recipient profile for an existing user |
| `GET` | `/profile/{user_id}` | Get full profile (blood type, DOB, emergency contacts, …) |
| `GET` | `/dashboard/{user_id}` | Get dashboard data for a user |

**Submit a donor form**

```bash
curl -X POST http://localhost:8000/submit-donor-form \
  -H "Content-Type: application/json" \
  -d '{
    "email": "ali@example.com",
    "bloodGroup": "O+",
    "dob": "1995-06-15",
    "gender": "male",
    "mobile": "0300-1234567",
    "name1": "Sara Khan", "phone1": "0311-9876543",
    "email1": "sara@example.com", "relation1": "Sister"
  }'
```

### Emergency Contacts

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/add-emergency-contacts/{user_id}` | Add up to 2 emergency contacts for a user |

```bash
curl -X POST http://localhost:8000/add-emergency-contacts/1 \
  -H "Content-Type: application/json" \
  -d '[{"name": "Sara Khan", "phone": "0311987654", "email": "sara@example.com", "relation": "Sister"}]'
```

## Project Structure

```
Khooni-Backend/
├── main.py          # FastAPI app, routes, and business logic
├── models.py        # SQLAlchemy ORM models (User, Donor, Receiver, EmergencyContact)
├── schema.py        # Pydantic request/response schemas
├── database.py      # Database engine and session factory
├── compose.yml      # Docker Compose file for PostgreSQL
└── requirements.txt # Python dependencies
```

## Getting Help

- **FastAPI docs** – https://fastapi.tiangolo.com/
- **SQLAlchemy docs** – https://docs.sqlalchemy.org/
- **Issues** – Open a GitHub issue in this repository for bug reports or feature requests

## Contributing

1. Fork the repository and create a feature branch (`git checkout -b feature/my-feature`).
2. Make your changes and ensure the server starts without errors (`uvicorn main:app --reload`).
3. Open a pull request describing what you changed and why.

## Maintainer

Developed and maintained by [GuessWho121](https://github.com/GuessWho121).
