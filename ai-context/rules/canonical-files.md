# canonical-files.md — Canonical files (template)

> Template/prompt. The AI replaces this with the final list, based on what **exists and follows the project's current pattern**. If there's no good example of something, omit it.

## Instructions for the AI

List the repo files that serve as a **template** when creating something new of each type. Point to each one with a relative path (`src/services/user-service.ts`, `Project.API/Controllers/AdminUsersController.cs`).

For a backend project, consider listing:

- Controller / Route handler / API entry point.
- Service / Use case / Application service.
- Repository / Data access / ORM model.
- Input validator.
- Mapper / DTO transformer.
- Entity / Domain model (especially if it's rich).
- Background job / Worker / Scheduled task.
- WebSocket handler / Hub (if any).

For a frontend project, consider listing:

- A reference reusable component (typed props, style pattern).
- Page / route handler.
- Custom hook.
- Service / API client.
- Store / state slice (if there's global state management).
- Layout / page template.
