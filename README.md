# AI Context Boilerplate

Templates to standardize the context that each repository offers to AI agents (Claude Code, Cursor, Copilot, Windsurf, Gemini CLI, Codex, etc.).

This repository does **not** contain ready-made rules. It contains **prompts/templates** that, once copied into a target repository and executed by the AI, generate the context documentation specific to that repository.

<img width="1845" height="1833" alt="image" src="https://github.com/user-attachments/assets/6c013ccd-89fd-4b84-a451-56f7b8d3e06f" />


## Why this repo exists

Today each IDE/agent reads a different file:

- Claude Code reads `CLAUDE.md`.
- Cursor reads `.cursorrules` (and, in recent versions, `AGENTS.md`).
- GitHub Copilot reads `.github/copilot-instructions.md`.
- Windsurf reads `.windsurfrules`.
- Gemini CLI reads `GEMINI.md`.
- OpenAI Codex, OpenCode and other agents read `AGENTS.md`.

Without standardization, each dev writes their own context, in different formats, with different depth. This boilerplate solves that with a single structure, organized as a **context layer** (`ai-context/`) indexed by one always-on file:

- `AGENTS.md` — agent behavior (short index, always read). It is the **source of truth**. It lives at the root and is read natively by Codex, Cursor and OpenCode; the other tools reach it via a bridge.
- `ai-context/ARCHITECTURE.md` — how the system is structured (read on the first interaction).
- `ai-context/CONVENTIONS.md` — code conventions and reusable patterns (read on the first interaction).
- `ai-context/RUNBOOK.md` — how to build, run locally, test and migrate (read on the first interaction).
- `ai-context/rules/` — detailed, modular behavior (read on the first interaction).
- Bridge files (`CLAUDE.md`, `GEMINI.md`, `.cursorrules`, `.windsurfrules`, `.github/copilot-instructions.md`) that point to `AGENTS.md`.

Regardless of the IDE/agent the dev uses, the context the AI receives is the same.

Loading is **by instruction**, not by auto-load: `AGENTS.md` tells the AI to read the `ai-context/` layer. It works the same in any agent, without depending on a tool-specific folder auto-load.

## Structure

```
ai-context-boilerplate/
├── README.md                                    ← you are here
├── AGENTS.md                                    ← source of truth: always-on behavior index
├── CLAUDE.md                                    ← bridge for Claude Code → AGENTS.md
├── GEMINI.md                                    ← bridge for Gemini CLI → AGENTS.md
├── .cursorrules                                 ← bridge for Cursor → AGENTS.md
├── .windsurfrules                               ← bridge for Windsurf → AGENTS.md
├── .github/
│   └── copilot-instructions.md                  ← bridge for GitHub Copilot → AGENTS.md
└── ai-context/
    ├── ARCHITECTURE.md                          ← architecture template
    ├── CONVENTIONS.md                           ← conventions template
    ├── RUNBOOK.md                               ← how to build / run / test
    └── rules/                                   ← detailed, modular behavior
        ├── task-approach.md
        ├── definition-of-done.md
        ├── canonical-files.md
        ├── security.md
        └── commit-format.md
```

## Workflow to create the documentation for a new repo

1. **Copy the boilerplate into the target repository.** Keep the exact folder structure and names above. `AGENTS.md` and the bridges live at the root (and in `.github/`); the context layer lives in `ai-context/`.

   ```
   # example on Windows (PowerShell), from the target repo directory:
   Copy-Item -Recurse C:\path\to\ai-context-boilerplate\* .\
   ```

2. **Open the repository in your preferred AI agent** (Claude Code, Cursor, Codex, etc.).

3. **Ask the AI to generate the final files:**

   > Generate the final content of `AGENTS.md`, of the files in `ai-context/rules/`, and of `ai-context/ARCHITECTURE.md`, `ai-context/CONVENTIONS.md` and `ai-context/RUNBOOK.md` for this repository. Each file contains its own generation instructions. Replace the content with the result, reading the repository's code to fill in each section.

4. **Review the result.** The AI will extract a lot correctly on its own, but some points need a human touch (e.g., gotchas only you know, review priorities, decisions still under discussion).

5. **Commit the finalized files + the bridges.**

   ```
   git add AGENTS.md ai-context/ CLAUDE.md GEMINI.md .cursorrules .windsurfrules .github/copilot-instructions.md
   git commit -m "chore: add AI context layer"
   ```

From then on, any AI the team uses in the repository reads `AGENTS.md`, which points to the `ai-context/` layer — and the context is uniform.

## Template principles

The templates were designed around four principles:

**Separation of concerns.** Each file has a clear purpose. `AGENTS.md` is essential behavior (always-on). `ai-context/rules/` is detailed behavior. `ARCHITECTURE.md` is system structure. `CONVENTIONS.md` is code patterns. `RUNBOOK.md` is how to run the project. Nothing is duplicated between them. When a section starts growing too much in one file, that's a sign it belongs in another.

**Always-on vs on-demand.** `AGENTS.md` is read **on every interaction** (it's part of the agent's system prompt). It must be short (ideally 80–150 lines). The `ai-context/` layer is read **once at the start of the session** — it can be denser.

**IDE/agent agnostic.** None of the templates depend on a specific tool. The bridges ensure that any agent using the repository reads the same source of truth, and the `ai-context/` layer is loaded by instruction from `AGENTS.md`.

**Explicit feedback loop.** When the agent generates code that violates a convention, the rule is to update `ai-context/CONVENTIONS.md` in the same PR. The documents grow with the team's learning.

## About the style of the templates

The templates take into account some practices observed in teams with more experience using SDD (Spec-Driven Development) and context standardization:

- Direct prose. No tables (they consume tokens and don't help the model).
- Positive language. "Use X" instead of "don't use Y".
- No historical versioning in the text. Write "it is X", not "it used to be Y, now it's X".
- No long lists of anti-patterns. When you need to flag a mistake, show right and wrong side by side.
- Bold reserved for 3 or 4 critical points across the whole document.

These patterns apply both to the templates here and to the files generated from them.

## Updating the templates

Changes here affect **the next time** a repository is created or regenerated from the boilerplate. They don't retroactively affect repos that already consumed the template.

For a change here to propagate:

1. Edit the relevant template (`AGENTS.md`, the files in `ai-context/rules/`, `ai-context/ARCHITECTURE.md`, `ai-context/CONVENTIONS.md` or `ai-context/RUNBOOK.md`).
2. Let the team know. Existing repos may choose to regenerate their docs in the next work cycle.
3. PRs here require mandatory senior review.

## Maintainers

The maintainers of this boilerplate.

## Quick links

- [`AGENTS.md` template](./AGENTS.md)
- [`ARCHITECTURE.md` template](./ai-context/ARCHITECTURE.md)
- [`CONVENTIONS.md` template](./ai-context/CONVENTIONS.md)
- [`RUNBOOK.md` template](./ai-context/RUNBOOK.md)
- [Detailed rules (`ai-context/rules/`)](./ai-context/rules/)
