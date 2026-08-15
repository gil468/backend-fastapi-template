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

---

# Claude Code Development Cheat Sheet

A practical cheat sheet for useful **Claude Code commands, workflows, and habits** during day-to-day development.

> Note: Command availability can depend on your Claude Code version and configuration.

---

## 1. Essential Slash Commands

### `/help`

Show available commands and usage information.

```text
/help
```

Use it when:

- You forget a command.
- You want to check what your installed version supports.

---

### `/compact`

Compress the current conversation context.

```text
/compact
```

Useful when:

- The conversation has become long.
- Claude is carrying lots of old context that is no longer important.
- You want to reduce context usage while keeping the important information.

You can also give Claude a hint about what to preserve:

```text
/compact Focus on the current authentication implementation and the API design decisions.
```

**Good habit:** Compact after finishing a major feature or debugging session before starting something unrelated.

---

### `/clear`

Clear the current conversation context and start fresh.

```text
/clear
```

Use it when:

- You are switching to a completely different task.
- The existing context is confusing Claude.
- You want a clean start.

---

### `/resume`

Resume a previous Claude Code session.

```text
/resume
```

Useful when you stopped working and want to continue from an earlier session.

---

### `/status`

Display information about the current Claude Code session.

```text
/status
```

Useful for quickly checking the current environment and session state.

---

### `/cost`

Show token/cost information for the current session.

```text
/cost
```

Useful when monitoring usage during long development sessions.

---

### `/model`

Check or change the model used by Claude Code.

```text
/model
```

Use this when you want to switch models based on the task, such as using a stronger model for architecture and a faster one for simpler tasks.

---

### `/init`

Ask Claude Code to analyze the repository and create project instructions.

```text
/init
```

Typically used when starting to work with Claude Code in an existing codebase.

The goal is to give Claude persistent project context, such as:

- Project architecture
- Important commands
- Coding conventions
- Testing instructions
- Repository structure

This is one of the best commands to run when first introducing Claude Code to a new repository.

---

## 2. Project Instructions: `CLAUDE.md`

One of the most useful ways to improve Claude Code is to provide persistent instructions through a `CLAUDE.md` file.

Example:

````md
# Project Instructions

## Stack

- Python 3.12
- FastAPI
- PostgreSQL
- Redis

## Commands

Run tests:

```bash
uv run pytest
```
````

Run linting:

```bash
uv run ruff check .
```

## Rules

- Use type hints.
- Follow existing project patterns.
- Do not introduce new dependencies without asking.
- Add tests for new business logic.
- Run linting and tests before considering a task complete.

````

Good things to include:

- Tech stack
- Project structure
- Build commands
- Test commands
- Linting/formatting commands
- Architecture rules
- Naming conventions
- Important constraints

**Think of `CLAUDE.md` as onboarding documentation specifically for Claude.**

---

## 3. Very Useful Development Prompts

### Understand the codebase

```text
Explain the architecture of this project. Start by identifying the main entry points, modules, data flow, and external dependencies.
````

---

### Understand a specific feature

```text
Trace how user authentication works end-to-end, from the incoming request to database access.
Do not make changes yet.
```

The phrase **"Do not make changes yet"** is useful when you first want understanding before implementation.

---

### Ask Claude to create a plan first

```text
Analyze the task and create an implementation plan.
Do not modify any files until I approve the plan.
```

This is especially useful for:

- Large refactors
- Database changes
- Architecture changes
- Multi-file features

---

### Investigate before fixing

```text
Investigate the root cause of this bug.

1. Inspect the relevant code.
2. Explain the likely cause.
3. Propose a fix.
4. Do not modify files until you explain the plan.
```

---

### Ask for minimal changes

```text
Fix this issue with the smallest possible change.
Avoid unrelated refactoring.
```

Very useful when working in a production codebase.

---

### Preserve existing patterns

```text
Implement this feature by following the existing patterns already used in the repository.
Do not introduce a new architectural approach unless necessary.
```

---

## 4. A Good Feature Development Workflow

### Step 1 — Understand

```text
I need to implement <feature>.

First, inspect the repository and explain:
- Which files are relevant
- How the current implementation works
- What needs to change

Do not modify anything yet.
```

---

### Step 2 — Plan

```text
Create a step-by-step implementation plan.

Include:
- Files to modify
- New components/modules if needed
- API/database changes
- Potential edge cases
- Tests to add

Do not start implementing yet.
```

---

### Step 3 — Implement

```text
Implement the approved plan.

Requirements:
- Follow existing code conventions
- Keep changes focused
- Avoid unrelated refactoring
- Add/update tests where appropriate
```

---

### Step 4 — Review

```text
Review your own changes as if you were doing a production pull request.

Look for:
- Bugs
- Edge cases
- Security issues
- Performance issues
- Missing tests
- Unnecessary complexity

Fix any issues you find.
```

---

### Step 5 — Verify

```text
Run the relevant tests and linting.

If anything fails:
1. Investigate the failure.
2. Fix the issue.
3. Run the checks again.

Do not stop until the relevant checks pass or you are blocked.
```

---

## 5. Debugging Workflow

Instead of immediately saying:

```text
Fix this bug.
```

Try:

```text
Investigate this issue before making changes.

Observed behavior:
<describe the problem>

Expected behavior:
<describe expected behavior>

Please:
1. Reproduce or trace the issue.
2. Identify the root cause.
3. Explain the relevant execution flow.
4. Propose the smallest safe fix.
5. Implement it.
6. Add or update a regression test.
```

This usually produces more reliable results because Claude is encouraged to **understand first and modify second**.

---

## 6. Useful Git Workflows

### Review current changes

```text
Review the current git diff.

Explain:
- What changed
- Potential problems
- Missing tests
- Whether anything appears unrelated to the intended task
```

---

### Before committing

```text
Review all current changes as a senior engineer.

Check:
- Correctness
- Error handling
- Edge cases
- Security
- Performance
- Code quality
- Test coverage

Only suggest changes that are genuinely valuable.
```

---

### Generate a commit message

```text
Review the current git diff and suggest a concise conventional commit message.
```

Example output:

```text
feat(auth): add refresh token rotation
```

---

## 7. Refactoring Prompts

### Safe refactor

```text
Refactor this code without changing its external behavior.

Requirements:
- Preserve the existing API
- Keep the change focused
- Do not introduce unnecessary abstractions
- Add or update tests if needed
```

---

### Before a large refactor

```text
Analyze this area for refactoring opportunities.

First provide:
1. Current problems
2. Proposed design
3. Benefits and trade-offs
4. Migration steps
5. Risks

Do not modify code yet.
```

---

## 8. Testing Prompts

### Add tests

```text
Analyze the existing test patterns and add tests for this feature.

Cover:
- Happy path
- Important edge cases
- Failure scenarios

Follow the existing testing style in the repository.
```

---

### Find missing test coverage

```text
Review this implementation and identify the most important missing test cases.

Prioritize realistic failure modes over trivial tests.
```

---

### Debug a failing test

```text
Investigate why this test is failing.

Do not change the test or production code immediately.
First explain whether the problem is:
- A real bug
- An incorrect test expectation
- An environment/configuration issue
- A flaky test

Then propose the appropriate fix.
```

---

## 9. Efficient Context Management

### When the context gets too large

Use:

```text
/compact
```

A useful pattern:

```text
We finished the authentication feature. Compact the conversation while preserving:
- Authentication architecture
- Important implementation decisions
- Remaining TODOs
- Known issues
```

Then:

```text
/compact
```

---

### When starting an unrelated task

Use:

```text
/clear
```

Then give Claude fresh context for the new task.

---

### For large tasks

Do **not** dump everything into one giant prompt.

Prefer:

```text
Understand → Plan → Implement → Test → Review
```

This generally gives you more control and makes mistakes easier to catch.

---

## 10. High-Value Prompt Patterns

### "Do not modify yet"

```text
Analyze the code first. Do not modify any files yet.
```

Use when:

- You are unfamiliar with the code.
- The task is risky.
- You want to review the approach first.

---

### "Follow existing patterns"

```text
Find similar implementations in the repository and follow their patterns.
```

Use when:

- Adding a new endpoint
- Adding a service
- Adding tests
- Implementing validation
- Adding a database migration

---

### "Smallest possible change"

```text
Make the smallest possible change that correctly solves the problem.
Avoid unrelated cleanup or refactoring.
```

Excellent for bug fixes.

---

### "Explain trade-offs"

```text
Compare the possible approaches and explain the trade-offs before choosing one.
```

Useful for architecture decisions.

---

### "Challenge the assumptions"

```text
Before implementing, challenge my proposed solution and point out potential problems or simpler alternatives.
```

Useful when you already have a design in mind.

---

### "Production review"

```text
Review this as production code.
Be critical and focus on real risks rather than stylistic preferences.
```

---

## 11. Recommended `CLAUDE.md` Rules

A solid baseline:

```md
# Development Guidelines

## Before Coding

- Inspect existing patterns before introducing new ones.
- Prefer simple solutions over unnecessary abstractions.
- Ask for clarification if requirements are ambiguous.

## During Coding

- Keep changes focused on the requested task.
- Do not modify unrelated files.
- Follow existing naming and architectural conventions.
- Prefer readable code over clever code.
- Do not add dependencies unless necessary.

## Testing

- Add tests for new business logic.
- Update existing tests when behavior intentionally changes.
- Run relevant tests after implementation.

## Completion

Before declaring a task complete:

1. Review the diff.
2. Run tests.
3. Run linting/formatting.
4. Check for edge cases.
5. Summarize the changes.
```

---

## 12. Quick Command Reference

| Command    | What it does                             | When to use it                 |
| ---------- | ---------------------------------------- | ------------------------------ |
| `/help`    | Shows available commands                 | You forgot a command           |
| `/compact` | Compresses conversation context          | Long session / context cleanup |
| `/clear`   | Starts with a clean context              | Switching to unrelated work    |
| `/resume`  | Resumes a previous session               | Continuing previous work       |
| `/status`  | Shows session information                | Checking current session       |
| `/cost`    | Shows usage/cost information             | Monitoring long sessions       |
| `/model`   | Check/change the model                   | Choosing a model for the task  |
| `/init`    | Initializes project context/instructions | Starting in a new repository   |

---

# My Recommended Daily Workflow

## Starting in a new repository

```text
/init
```

Then review and improve the generated `CLAUDE.md` if necessary.

---

## Starting a medium/large feature

```text
1. Understand the relevant code.
2. Create a plan.
3. Wait for approval if the change is significant.
4. Implement.
5. Run tests and linting.
6. Review the git diff.
7. Do a final production-style code review.
```

---

## When Claude starts losing context

```text
/compact
```

---

## When switching to something completely unrelated

```text
/clear
```

---

# Golden Rules

1. **Understand before changing.**
2. **Plan before large implementations.**
3. **Keep changes focused.**
4. **Ask Claude to follow existing patterns.**
5. **Use `/compact` instead of carrying unnecessary old context.**
6. **Review the git diff before committing.**
7. **Ask Claude to review its own work critically.**
8. **Run tests and linting before declaring success.**
9. **Prefer small, incremental tasks over one giant prompt.**
10. **Treat Claude as a very fast engineer that still needs clear requirements and code review.**
