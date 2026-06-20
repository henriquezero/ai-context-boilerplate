# CONVENTIONS.md — Template

> **This file is a template/prompt**, not a project's final CONVENTIONS.md.
>
> When copied into a target repository, it must be **executed by the AI** to generate that repository's real `CONVENTIONS.md`, based on the patterns of the existing code.

---

## Instructions for the AI to generate the final CONVENTIONS.md

Replace the entire content of this file (including this instructions section) with the repository's final `CONVENTIONS.md`, following the spec below.

### Purpose of CONVENTIONS.md

It describes **how to write code in this project**. Naming conventions, reusable patterns (Repository, Service, Mapper, Validator, Hook, Component...), database/storage rules, logging rules.

Every pattern described here must have a concrete example in the code — point to the canonical file so the reader can confirm.

It does not cover the **system architecture** (`ARCHITECTURE.md`) nor **agent behavior** (`AGENTS.md`).

### Quality criteria

- Ideal size: 150 to 300 lines. Beyond that, consider whether there are duplicated patterns or whether some pattern deserves a dedicated file (`ai-context/patterns/repository.md`).
- Direct prose with short code snippets. Snippets must be authentic to the project (extracted from real code), not invented.
- No naming tables (e.g., "classes in PascalCase, methods in camelCase"). Instead, describe the convention in prose and show examples from the code.
- Where there's "right vs wrong", show both side by side in code blocks.
- Positive language: "use X" instead of "don't use Y".

### Expected structure

Include the sections that apply to the project. Omit the ones that don't make sense (e.g., a backend has no Hook pattern; a frontend has no SQL Repository pattern).

#### Language / runtime conventions

Conventions specific to the ecosystem. Discover the patterns by observing the existing code, not by imposing a generic convention. Examples of what goes here:

- Naming: how classes, methods, properties, variables, constants, files and folders are named. Use prose with examples from the project, not tables.
- Async/await: conventions (`Async` suffix in .NET, the standard in Node/Python).
- Typing: type rules (TypeScript strict, no `any`, use of `interface` vs `type`).
- Imports: order and organization.
- Error handling: try/catch, `Result`/`Either`, typed exceptions, panics.

#### Project patterns

For each reusable architectural pattern in the project, a subsection. Structure each subsection like this:

1. Pattern name.
2. Where it lives (folder path).
3. When to use it.
4. Specific conventions (signature, dependencies, return).
5. Real code snippet from the project.
6. Canonical file to base it on.

Examples of the kind of pattern that goes here (include only what exists in the project):

For a backend project:

- **Error-return pattern**: Result pattern, Either, typed exceptions. Where it's implemented.
- **Repository**: how it accesses the database, error-handling pattern, parameterization, mapping.
- **Service / Use Case**: what it orchestrates, what it returns.
- **Validator**: library used (FluentValidation, zod, Pydantic), where it lives, what it validates.
- **Mapper / DTO transformer**: structure, common helpers.
- **Controller / Route handler**: mandatory attributes, return pattern, access to the current user.
- **Domain entity** (if there's a rich model): factory methods, encapsulation, business methods.
- **Background job / Worker**: registration, schedule, idempotency, cleanup.
- **WebSocket hub / handler**: authentication, access to the context.

For a frontend project:

- **Component**: structure, typed props, export pattern, server vs client.
- **Custom hook**: conventions (`use` prefix, return as object vs tuple).
- **Service / API client**: how it calls the backend, error handling, response types.
- **State management**: store per feature, actions/reducers/atoms pattern.
- **Form handling**: library, validation schema, submission pattern.
- **Style / theming**: Tailwind, CSS-in-JS, CSS modules — specific conventions (e.g., avoid arbitrary values when there's a class).
- **Routing**: dynamic routes, layouts, loading and error boundaries.

#### Database (if applicable)

- Naming convention for tables and columns (discover the current pattern in the project, don't invent).
- Migration pattern: where they live, how they're versioned, rules about editing a migration already applied.
- Types: how IDs (`int`, `bigint`, `uuid`), dates, soft delete, auditing (`created_at`, `updated_at`, `created_by`).
- Indexing: naming conventions, when to create one.
- Constraints: rules about triggers, stored procedures, computed columns.

#### Storage / files (if applicable)

Patterns for upload/download, storage folder organization, naming, retention.

#### Logging

- Stack used (already described in ARCHITECTURE.md — here focus on **how the dev uses it**).
- Structured logging: how to pass parameters (template + values, not interpolation).
- Real snippet from the project.
- What **not** to log: sensitive data, large payloads, repetition of what's already enriched by middleware (TenantId, UserId, CorrelationId).

#### Configuration

How to add a new configuration value:

1. Where to declare the type (POCO, interface, schema).
2. Where to add the value (secret manager, .env, appsettings).
3. How to register it (DI, IOptions, getEnv).
4. How to inject it.

#### Localization and culture (if applicable)

- The application's default culture (forced locale? forced timezone?).
- How to format dates, numbers, currencies for the user.
- How to persist dates in UTC vs locale.
- I18n: library used, where translations live, key pattern.

### Discovery procedure

To generate a quality CONVENTIONS.md, explore the repository in this order:

1. Identify the stack and the project type (backend / frontend / fullstack / lib).
2. List patterns observable from the folder structure: is there a `services/`? `repositories/`? `mappers/`? `validators/`? `components/`? `hooks/`? Each such folder is a pattern candidate.
3. For each pattern candidate, read 2 or 3 representative files and identify:
   - Common structure (constructor, injected dependencies, decorators/attributes at the top).
   - Error-handling style.
   - Return pattern (Result, throw, void, response object).
   - Naming conventions inside the file.
4. Look for a "base" file or common helper (e.g., `SqlRepository.cs`, `BaseService.ts`, `useApi.ts`) — they usually reveal the mandatory pattern.
5. For database patterns: read 1 or 2 migrations and 1 or 2 queries to identify naming conventions.
6. For logging: look for `_logger.LogX`, `logger.info`, `log.debug` calls and see how they're built.
7. For configuration: find where DI registers services and where the settings are read.

### How to extract snippets

When showing a pattern example, prefer a **real snippet from the project** over invented code. Copy 5 to 15 relevant lines from the canonical file, with ellipses to omit non-essential parts. Always identify the source file in the comment or adjacent prose.

Example of how to present a snippet:

```
Pattern inside each Repository method (extracted from `Project.Infra/Repositories/UserRepository.cs`):

    public async Task<User> GetByIdAsync(int id)
    {
        try
        {
            RequireTenant();
            // ... query execution
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Failed to get user {UserId}", id);
            throw;
        }
    }

Points of the pattern: `RequireTenant()` at the start, structured log + rethrow in the catch.
```

### Writing style

- Each pattern has 1 paragraph of prose + 1 small snippet + 1 reference to the canonical file. More than that only if unavoidable.
- Use active voice.
- Don't duplicate information already in `ARCHITECTURE.md`. If the pattern depends on an architectural decision (e.g., the Result pattern depends on the choice to return errors as values), just reference it: "As described in ARCHITECTURE.md (Core architectural decisions)".
- Lists with long items lose their force. Break into prose when each item goes beyond 2 sentences.
