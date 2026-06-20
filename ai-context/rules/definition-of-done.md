# definition-of-done.md — Definition of done (template)

> Template/prompt. The AI replaces this with the project's final checklist.

## Instructions for the AI

Generate the checklist that every change must meet before being considered done — it applies when opening a PR or committing directly to a shared branch. Adapt to the project type. Always include:

- Build/lint/type-check pass with no new warnings.
- No secrets, no commented-out code, no debug prints.
- No real credentials in fixtures, no hardcoded production URLs.

Add as appropriate:

- Backend: a new endpoint shows up in Swagger/OpenAPI with the correct auth. A new migration tested on a recent copy of the staging database.
- Frontend: tested on mobile and desktop viewports. No `any` (TypeScript). Fetch following the project's pattern.
- Error messages returned to the client in the correct language, without exposing a stack trace.
