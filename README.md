## UV:

## Install UV

```bash
# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh
uv --version
```

## Create a New Project

```bash
uv init .
```

## Virtual Environments

For projects managed by UV, `uv sync` normally creates `.venv` automatically.

If you specifically want to create a virtual environment:

```bash
uv venv
```

Then activate it manually if needed:

```bash
# macOS / Linux
source .venv/bin/activate

# Windows
.venv\Scripts\activate
```

You can check which virtual environment (venv) uv is currently targeting by running uv python find.

```bash
uv python find
```

If you are inside a project with an active virtual environment, it will output something like: /path/to/your/project/.venv/bin/python.

## Add dependencies

```bash
uv add requests
uv add --dev pytest
```

# Remove a dependency

```bash
uv remove requests
```

## Develop

```bash
uv run main.py
uv run pytest
```

**Mental model:**  
`uv init` → create project  
`uv add` → manage dependencies  
`uv sync` → reproduce the environment  
`uv run` → execute code  
`uv.lock` → keep dependency versions reproducible

---

## Ruff:

A simple CI workflow can run:

```bash
uv run ruff check . && ruff format . --check
```

## Recommended Workflow

```bash
# Install as a dev dependency
uv add --dev ruff

# During development
uv run ruff check . --fix && ruff format .

# Before commit / CI
uv run ruff check . && ruff format . --check
```

## Commands to Remember

| Command                 | Purpose                                 |
| ----------------------- | --------------------------------------- |
| `ruff check .`          | Lint the project                        |
| `ruff check . --fix`    | Lint + automatically fix issues         |
| `ruff format .`         | Format Python code                      |
| `ruff format --check .` | Check formatting without changing files |
| `ruff rule <rule>`      | Explain a Ruff rule                     |
| `ruff config`           | Show configuration options              |
| `ruff --version`        | Show installed Ruff version             |
