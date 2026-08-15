# Ad Analytics Backend Guidelines

## Core Mindset & Constraints

- **KISS Principle:** Prefer clean, readable, simple, and direct solutions. Avoid over-engineering, unnecessary abstractions, design patterns that add bloat.
- **Production-Quality Simplicity:** Code must be clear, well-structured, robust, and correctly handle edge cases without being overly complex.

## Tech Stack

- Python 3.11+
- Framework: FastAPI
- Package & Environment Manager: uv
- Data Validation: Pydantic v2
- Linter / Formatter: Ruff (`ruff check`, `ruff format`)
- Testing: pytest (`pytest`)

## Code Style & Rules

- Use Async/Await for all FastAPI route handlers and DB operations.
- Always use type hints and Pydantic schemas for request/response validation.
- Keep route handlers light; move business logic to service modules (`app/api/`).
- Do not write unnecessary inline comments; keep functions modular and self-explanatory.
- Follow PEP 8 guidelines enforced by Ruff.

## Useful Commands (Always run via `uv`)

- Install dependency: `uv add <package-name>`
- Install dev dependency: `uv add --dev <package-name>`
- Sync/Create venv: `uv sync`
- Run server: `uv run main.py`
- Run tests: `uv run pytest`
- Run single test file: `uv run pytest tests/test_filename.py`
- Lint code: `uv run ruff check .`
- Format code: `uv run ruff format .`
