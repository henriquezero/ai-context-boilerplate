# AGENTS.md — Template

> **This file is a template/prompt**, not a project's final `AGENTS.md`.
>
> When copied into a target repository, it must be **executed by the AI** to generate that repository's real `AGENTS.md`, based on the code that lives there.

---

## How to use (for the human dev)

1. Copy these files to the root of your repository, keeping the structure:
   - `AGENTS.md` (this file) — source of truth, read by every AI.
   - `ai-context/ARCHITECTURE.md`, `ai-context/CONVENTIONS.md` and `ai-context/RUNBOOK.md`.
   - `ai-context/rules/` — detailed behavior rules.
   - Bridges: `CLAUDE.md`, `GEMINI.md`, `.cursorrules`, `.windsurfrules`, `.github/copilot-instructions.md`.
2. Open your preferred AI agent (Claude Code, Cursor, Codex, etc.) in the repository.
3. Ask the AI: **"Generate the final content of `AGENTS.md`, of the files in `ai-context/rules/`, and of `ai-context/ARCHITECTURE.md`, `ai-context/CONVENTIONS.md` and `ai-context/RUNBOOK.md`, following each template's instructions. Replace the content, reading the repository's code."**
4. Review the result. Adjust whatever is specific to your project.
5. Commit. The bridges don't need editing — they only point to `AGENTS.md`.

---

## Instructions for the AI to generate the final AGENTS.md

From here on, all content is instruction **for you (the AI)**. Replace the entire content of this file (including the two sections above) with the repository's final `AGENTS.md`, following the spec below. Then generate the `ai-context/rules/` files, following the generation instructions inside each one.

### Purpose of AGENTS.md

It is the **source of truth** for agent behavior in this repository, read by every AI tool — natively by Codex, Cursor and OpenCode; via a bridge by Claude Code, Gemini CLI, Copilot and Windsurf. It lives at the **root** because tools look for that name there.

It is an **always-on index**: it's part of the system prompt, so it must be short. It does not describe the system (`ai-context/ARCHITECTURE.md`), nor code patterns (`ai-context/CONVENTIONS.md`), nor the detailed behavior (`ai-context/rules/`). It points to those.

### Quality criteria

- Ideal size: 80 to 150 lines. Absolute maximum: 200. Beyond that, move content into `ai-context/rules/`.
- Direct prose, short lists. Avoid tables — they consume tokens and don't help the AI.
- Positive language: "use X" instead of "don't use Y".
- No "it used to be X, now it's Y" — just write "it is X".
- Bold reserved for at most 3 or 4 critical points in the whole document.

### Expected structure

Include the sections below, in this order. Adapt the titles to the project's tone, but keep the substance.

#### Header (1 to 2 sentences)

Say what the agent finds in this repository and reinforce: read before any action, on every interaction.

#### Reading on the first interaction of the session

List what the agent should read **once** at the start of the session:

- `ai-context/ARCHITECTURE.md` — architecture and system structure.
- `ai-context/CONVENTIONS.md` — code conventions and project patterns.
- `ai-context/RUNBOOK.md` — how to build, run locally, test and migrate.
- `ai-context/rules/` — detailed behavior rules (read the files in the folder).
- Other project documents if they exist (active specs, ADRs, domain glossary).

State explicitly: "Do not re-read in later interactions of the same session. Re-read only if the user signals that something changed."

#### Language in code

Recommended convention:

- Identifiers (variables, functions, classes, properties, files, folders, branches, database) in **English**.
- Logs in **English**.
- Commits in **English**, in conventional commits style.
- User-facing messages in **PT-BR** or the product's language. Discover the current pattern by reading the existing messages in the code.

#### Security (hard limits)

Non-negotiable rules. Adapt to the project type:

- Never commit secrets (tokens, passwords, API keys, connection strings with credentials).
- Never log sensitive data (passwords, tokens, personal data, private message content).
- For authorization, always cross-check request IDs with the authenticated user (and with the tenant, in multi-tenant systems). IDs coming from the client are never trustworthy on their own.
- Backend with raw SQL: always parameterized queries, never concatenation.
- Frontend: beware of XSS — escape/sanitize user content when rendering as markup.
- Multi-tenancy (if applicable): every query scoped by tenant, derived from the authenticated context.

#### Index of the `ai-context/` layer

A short map of what exists and when to consult each thing:

- `rules/` — detailed behavior, one topic per file: `task-approach.md` (how to approach a task), `definition-of-done.md` (what every change must meet before it's done), `canonical-files.md` (repo files to use as templates when creating something new), `security.md` (project-specific security, deepening the hard limits above), `commit-format.md` (commit message format). Read at session start.
- `RUNBOOK.md` — real build, dev, test and database commands. Consult when you need to run or test the project.

### Discovery procedure

To generate a quality AGENTS.md:

1. Read the `README.md` if there is one. Note the project's purpose.
2. List the root directory tree (up to 2 levels).
3. Identify the stack from the config files at the root (`*.csproj`/`*.sln`, `package.json`, `pyproject.toml`/`requirements.txt`, `go.mod`, `Cargo.toml`).
4. Note the language of the user-facing messages in the existing code.
5. Identify the security hard limits that apply (multi-tenant? raw SQL? frontend rendering user content?).

The detailed behavior (task approach, definition of done, canonical files, security, commit format) does **not** go here — it goes in `ai-context/rules/`. Generate those files — `task-approach.md`, `definition-of-done.md`, `canonical-files.md`, `security.md`, `commit-format.md` — following the generation instructions inside each one.
