# ARCHITECTURE.md — Template

> **This file is a template/prompt**, not a project's final ARCHITECTURE.md.
>
> When copied into a target repository, it must be **executed by the AI** to generate that repository's real `ARCHITECTURE.md`, based on the code.

---

## Instructions for the AI to generate the final ARCHITECTURE.md

Replace the entire content of this file (including this instructions section) with the repository's final `ARCHITECTURE.md`, following the spec below.

### Purpose of ARCHITECTURE.md

It describes **how the system is structured**. Layers, dependencies between modules, architectural decisions already made, external integrations, infrastructure. It lets a new dev (or an AI) understand the system in 10 minutes of reading.

It does not cover **code conventions** (that's `CONVENTIONS.md`) nor **agent behavior** (that's `AGENTS.md`).

### Quality criteria

- Ideal size: 120 to 250 lines. Beyond that, consider moving very specific details into other documents (`ai-context/integrations.md`, `ai-context/jobs.md`, etc.) referenced from here.
- Direct prose, no tables. ASCII diagrams only when they add real value (layer structure, request flow).
- Don't list every file in the project — describe the **components**, not inventories. Point to specific files only when they illustrate a concept.
- Mention real versions of libs and the runtime (discover them in the config files).

### Expected structure

Include the sections below. Omit the ones that don't apply to the project, and add others if the project has unique characteristics worth highlighting.

#### Overview (2 to 4 paragraphs)

What this system is, what problem it solves, who uses it, what the key characteristics are (multi-tenant? realtime? offline-first? batch processing?). Mention the runtime and the main stack at a high level.

#### Stack

A list of the stack with **real versions** discovered in the config files:

- Runtime / language (with version).
- Main framework.
- Database (and ORM/driver, if any).
- Significant libraries (auth, logging, queue, validation, etc.).

Focus on what carries weight in the architecture. Don't list every transitive dependency.

#### Layered structure / modules

How the code is organized. It can be:

- Classic **Clean Architecture** (Presentation, Application, Domain, Infrastructure) — common in backends.
- **Feature-based** (each feature has its folder with components, hooks, services) — common in frontends.
- **Hexagonal** with ports & adapters.
- Simple **Layered** (controllers/services/repositories without a rich domain).
- Another structure specific to the project.

Describe the role of each layer/module and the dependency rule between them. Show an ASCII representation if it helps.

#### Core architectural decisions

The "by design" decisions that shape the system. Examples of the kind of thing that goes here (include only what applies):

- Multi-tenancy: how the tenant is resolved, where the `IdTenant` column lives, how queries are filtered.
- Authentication: supported providers (local, OAuth, SSO, SAML), token format, custom claims.
- Authorization: declarative (attributes/decorators) vs imperative, custom attributes, role model.
- BFF / proxy: if the frontend calls a BFF instead of the backend directly, describe why.
- CQRS, Event sourcing, Saga: if applicable.
- Server Components vs Client Components (Next.js): the rule for when to use each.
- Rendering: SSR, SSG, ISR, CSR — when to use each.

Each decision: 1 to 3 paragraphs. Explain **why** it exists this way, not just **how**.

#### Background processing (if applicable)

Scheduled jobs, workers, message queues, schedulers. List each job with its schedule and purpose. Point to where jobs are registered and where the dashboard lives (if any).

#### Realtime (if applicable)

WebSockets, SignalR, Server-Sent Events. List the channels/hubs/topics and the purpose of each. Mention special authentication if any (e.g., JWT via query string).

#### External integrations

Third-party services consumed by the system. For each one, 1 to 2 sentences about what and why. Examples:

- Cloud providers (AWS S3, AWS Secrets Manager, Azure Key Vault).
- Payment, messaging, email APIs.
- SSO providers (Azure AD, Google, Okta).
- Embedded platforms (PowerBI, Tableau, Looker).

#### Observability

How the system is observed. Minimum coverage:

- Logging: stack used, format (structured JSON or text), output (console, file, external aggregator), retention, enrichment (TenantId, UserId, CorrelationId).
- Metrics (if any): Prometheus, CloudWatch, etc.
- Tracing (if any): OpenTelemetry, X-Ray, etc.
- Handling of uncaught exceptions (global handler).

#### Configuration

Where sensitive configuration lives and how it reaches the application:

- Secret manager (AWS Secrets Manager, Azure Key Vault, HashiCorp Vault, GitHub Actions secrets).
- Per-environment config files (`appsettings.{env}.json`, `.env.{env}`).
- Environment variables.

Point to the file/module responsible for loading the configuration at startup.

#### Typical flow

ASCII diagram (or numbered prose) of the path of a typical operation in the system. In a backend, it's usually an HTTP request: middleware → controller → service → repository → database → response. In a frontend, it can be: route → component → hook → service → API → render.

The goal is to give a bird's-eye view of "how a typical thing happens in this system".

### Discovery procedure

To generate a quality ARCHITECTURE.md, explore the repository in this order:

1. Identify the stack from the config files (`*.csproj`, `package.json`, `pyproject.toml`, etc.). Note versions.
2. List the directory tree of the root and the next 1 or 2 levels. Identify the layers/modules by the folder names.
3. Read the bootstrap file (`Program.cs`, `Startup.cs`, `main.ts`, `app/layout.tsx`, etc.). Note: registered services, middleware, auth configuration, database connection, job/hub registration.
4. Read 1 or 2 representative files from each layer to confirm the structure.
5. For multi-tenancy: search for `tenant`, `IdTenant`, `tenantId` in the code. See where it's resolved and where it's used.
6. For authentication: look for the login handler, the JWT/cookie configuration, and authorization attributes/decorators.
7. For external integrations: look at imports (using statements, requires), environment variables and config files — they reveal third-party SDKs.
8. For jobs: look for schedulers (Hangfire, Celery, BullMQ, node-cron, etc.) and classes/functions marked as a job.
9. For realtime: look for SignalR Hub, Socket.io, ws, sse.

Cite specific files in the generated document when it helps the reader verify.

### Writing style

- Prose in paragraphs. Short lists only where the structure is genuinely parallel.
- State decisions in active voice: "Multi-tenancy is by design", not "It was decided that the system would be multi-tenant".
- Point to files with a relative path: `Project.API/Program.cs`, `src/services/auth.ts`.
- Use code blocks only for structural representations (folder tree, ASCII flow, small config snippet).
