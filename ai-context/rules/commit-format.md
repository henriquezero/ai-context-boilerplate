# commit-format.md — Commit message in Markdown

> Example and convention for this standard's commit message. Adapt it to your project's task tracker and commit types.

The commit message is the final artifact of the change — the equivalent of the closing `.md` of Spec-Driven Development, but versioned in the Git history instead of in a loose file. Each commit carries: the **task** that originated the change, what was done and why, and how it was verified. Anyone reading `git log` (or the commit view on GitHub/GitLab, where the body renders as Markdown) understands the change without opening the PR.

## Format

**Subject** (first line): a conventional commit in plain text, imperative, up to ~72 characters. Don't use Markdown here — it's what shows up in `git log --oneline` and what changelog/release tools parse.

```
<type>(<scope>): <short imperative summary>
```

Types: `feat`, `fix`, `refactor`, `chore`, `docs`, `test`, `perf`, `build`, `ci`.

**Body** (after a blank line): Markdown. Use the sections below, in this order; omit the ones that don't apply.

- `## Task` — task id and title (with a link, if any).
- `## Summary` — 1 to 3 sentences: what changed and why.
- `## Changes` — bullets per file or area with what was done.
- `## Decisions` — non-obvious design choices (omit if trivial).
- `## Notes` — breaking changes, follow-ups, what was left out of scope.

**Footer** (trailers, after a blank line): key-value pairs that tools read. Use `Closes #456` when the commit closes the issue.

```
Task: PROJ-123
Refs: #456
```

**Language**: subject and body in English, aligned with the commit rule in `AGENTS.md`. If the team prefers the body in the product's language, switch — just keep one consistent choice in the repository. UI messages quoted in the body can be reproduced in their original language.

## Example

```
feat(reports): add CSV export to the reports listing

## Task
PROJ-142 — Export the reports listing to CSV

## Summary
Adds an "Export CSV" button to the reports listing that downloads the
currently filtered rows, reusing the active filter and sort without an
extra fetch.

## Changes
- `src/app/api/reports/export/route.ts`: new BFF route that takes the same
  query params as the listing and streams back `text/csv`.
- `src/services/reportService.ts`: `exportReportsCsv(token, query)`.
- `src/components/Reports/ReportsToolbar.tsx`: button + loading state.
- `src/utils/csv.ts`: `toCsv` helper (escapes quotes and newlines).

## Decisions
Stream the response instead of building the whole CSV in memory — the
listing can have tens of thousands of rows. CSV escaping is hand-rolled
(no new lib) because the required format is simple.

## Notes
- Workspaces export is out of scope for this task.
- Follow-up: CSV pagination above 100k rows (separate task).

Task: PROJ-142
Refs: #456
```

## Why this way

- The short plain-text subject keeps `git log --oneline`, automatic changelogs and conventional-commit parsing working.
- The Markdown body becomes navigable documentation on GitHub/GitLab and survives in the history even when the PR is deleted.
- The `Task` inside the commit ties code to the requirement without depending on an external platform.
- The verification section records that the change was tested — not just claimed.

Keep the body proportional to the change: a one-line `fix` skips most sections; a large feature uses them all.
