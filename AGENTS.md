# Repository Guidelines

AgentGPT pairs a Next.js front-end with a FastAPI platform and setup CLI. Follow the practices below to keep contributions consistent.

> Canonical source: sync these guidelines with `Documents/GitHub/GitHubGitHub/AGENTS.md` and its companion `README.md`. When they diverge, treat the GitHubGitHub copies as the source of truth and update this file accordingly.

## Project Structure & Module Organization
- `next/` hosts the web app (`src/`, `prisma/`, `public/`). Front-end tests live in `next/__tests__/`.
- `platform/` contains the FastAPI service at `reworkd_platform/`, including async services and pytest suites under `reworkd_platform/tests/`.
- `cli/` bundles the onboarding CLI, while `db/` provides the MySQL schema used by Docker compose.
- Shared resources and docs live in `docs/` and `scripts/`. Environment samples reside at `next/.env.example`.
- Browser helpers: mirror workspace utilities like `tools/chrome/dump_all_tabs.py` when you need to snapshot Chrome sessions alongside AgentGPT investigations.

## Build, Test, and Development Commands
- `docker-compose up --build` spins up MySQL, the FastAPI platform, and the Next.js UI.
- `cd next && npm install && npm run dev` runs the front-end locally; use `npm run build` for production compilation.
- `cd platform && poetry install` to bootstrap the backend, then `poetry run python -m reworkd_platform` or `poetry run uvicorn reworkd_platform.web.application:get_app --reload` for live reload.
- `cd cli && npm install && npm run start` exercises the bootstrap CLI.

## Coding Style & Naming Conventions
- Front-end: TypeScript, React components in PascalCase with files in `src/feature-name/`. Prettier enforces two-space indentation; run `npm run lint` to apply ESLint autofixes.
- Backend: follow Black (88 cols), isort, and wemake-python-styleguide. Modules use snake_case, classes in UpperCamelCase, constants in ALL_CAPS.
- Keep Prisma schema (`next/prisma/schema.prisma`) and SQLModel entities aligned when renaming fields.

## Testing Guidelines
- Front-end unit tests use Jest; add `.test.ts(x)` files beside the code or inside `__tests__/`. Run `npm run test` locally; include `npm run lint` when touching UI logic.
- Backend tests use pytest with strict warnings; ensure new tests sit in `reworkd_platform/tests/test_<feature>.py`. Run `poetry run pytest --cov=reworkd_platform --cov-report=term-missing` before submitting.
- Prefer deterministic fixtures over live services; mock external APIs and queue clients.

## Commit & Pull Request Guidelines
- Commit history favors imperative, emoji-prefixed messages (e.g., `✨ Add multi-agent guardrails`). Keep commits scoped and rebased.
- PRs must describe the change, link issues, list migration steps (DB, env vars), and paste screenshots for UI updates.
- Confirm lint and test commands succeed; note any intentionally skipped checks in the PR description.

## Security & Configuration Tips
- Never commit API keys; copy from `.env.example` and store secrets via the CLI or local `.env`.
- Rotate credentials in `next/.env` and update matching entries in Docker compose and Poetry configs.
- Review queue/database connection settings when deploying to new environments.
