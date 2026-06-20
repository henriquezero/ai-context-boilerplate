# RUNBOOK.md — How to run the project (template)

> **This file is a template/prompt**, not a project's final RUNBOOK.
>
> When copied into a target repository, it must be **executed by the AI** to generate the real RUNBOOK, based on the project's scripts and configuration.

---

## Instructions for the AI to generate the final RUNBOOK

Replace the entire content of this file with the repository's RUNBOOK. The goal is to give the agent (and the new dev) the exact commands to run the project locally, without guessing. It's the document that saves the most AI time.

### Purpose

Lists the project's real commands for setup, build, run locally, test, lint/type-check and database. Copy-pasteable commands, not generic descriptions.

### Quality criteria

- Real commands, extracted from `package.json` (scripts), `Makefile`, `*.csproj`, `pyproject.toml`, `docker-compose.yml` or the README. Don't invent.
- Show the happy path first; variations later.
- Include **how to run a single test** — the agent uses this constantly in its fix loop.

### Expected structure

#### Prerequisites

Runtime/SDK and version (discover it in the config; align with `ai-context/ARCHITECTURE.md`), package manager, Docker if any, required accounts/credentials.

#### Initial setup

Steps from scratch after cloning: install dependencies, copy `.env.example` to `.env` and where the secrets come from, bring up the local infra (`docker compose up -d`), run migrations/seed.

#### Run locally

Command to bring up the application in dev mode (and each service, if there's more than one). Port/URL and health check to confirm it's up.

#### Tests

Command to run everything, to run a single file and to run **a single test**. Where to see coverage, if any.

#### Quality

Lint, format and type-check commands — the same ones CI runs.

#### Database

How to create and run a migration, seed and reset the local database.

### Discovery procedure

1. Read the scripts in `package.json`, or the `Makefile`, or equivalent targets.
2. Look for `docker-compose.yml` / `compose.yaml` for the local infra.
3. Check the CI pipeline (`.github/workflows/`, `azure-pipelines.yml`) — it reveals the canonical build/test/lint commands.
4. Find the `.env.example` and the per-environment config files.
5. Identify the migration tool (EF Core, Prisma, Alembic, Flyway...) and its command.

Cite the exact commands. If something doesn't exist in the project, omit the section.
