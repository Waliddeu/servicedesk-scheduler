# Service Desk Scheduler

A Django app that builds weekly shift schedules for a service desk team. It uses Google OR-Tools to assign agents to shifts so that opening hours are always covered, while respecting each agent's weekly hours limit and skipping Polish public holidays.

> **Status:** early development (Sprint 1 — `v0.1`). Expect breaking changes.

## Features

**Sprint 1 (in progress)**
- Agents, shifts and schedule entries managed through Django admin
- Working-day calculation that skips weekends and Polish public holidays
- Constraint solver: minimum staff per shift, one shift per agent per day, weekly hour limits
- One command to generate and save a week's schedule

**Planned**
- Background schedule generation with Celery + Redis
- Agent preferences and time-off requests
- Fair distribution of early/late shifts
- A web UI beyond Django admin

## Tech stack

- Python 3.13
- Django
- [OR-Tools](https://developers.google.com/optimization) (CP-SAT solver)
- [holidays](https://pypi.org/project/holidays/)
- pytest + pytest-django
- Celery + Redis (planned)

## Requirements

- **Python 3.11–3.14, 64-bit** (3.13 recommended).
  OR-Tools only publishes wheels for these versions. On Windows, use the Python from [python.org](https://www.python.org/downloads/windows/), not the MSYS2/MinGW build: OR-Tools has no wheels for MSYS2 Python.
- Git

## Getting started

```bash
# 1. Clone
git clone https://github.com/<your-username>/servicedesk-scheduler.git
cd servicedesk-scheduler

# 2. Create and activate a virtual environment
py -3.13 -m venv .venv            # Windows
python3.13 -m venv .venv          # Linux / macOS

source .venv/Scripts/activate     # Windows (Git Bash)
source .venv/bin/activate         # Linux / macOS

# 3. Install dependencies
pip install -r requirements.txt

# 4. Configure environment variables
cp .env.example .env              # then edit .env

# 5. Set up the database
python manage.py migrate
python manage.py createsuperuser
```

### Environment variables

| Variable            | Description                              | Example                    |
|---------------------|------------------------------------------|----------------------------|
| `DJANGO_SECRET_KEY` | Django secret key. Never commit it.      | `change-me`                |
| `DJANGO_DEBUG`      | Debug mode (`True` only for development) | `True`                     |
| `REDIS_URL`         | Redis connection (planned, for Celery)   | `redis://localhost:6379/0` |

## Usage

```bash
# Load demo data: 5 agents and 2 shifts
python manage.py seed_demo

# Generate the schedule for the week starting on a given Monday
python manage.py generate_schedule --week 2026-11-16

# Run the development server, then open http://127.0.0.1:8000/admin/
python manage.py runserver
```

## Running tests

```bash
pytest
```

Tests also run automatically on every push and pull request via GitHub Actions.

## Project workflow

- `v1` is the default branch and always holds working code.
- Work happens on short-lived branches named after issues, e.g. `feature/10-minimal-solver`.
- Changes are merged through pull requests; writing `Closes #10` in the PR description closes the issue automatically.
- Planning lives in GitHub Issues, grouped by sprint milestones.

## License

Released under the MIT License. See [LICENSE](LICENSE).
