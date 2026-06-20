# security.md — Security (template)

> Template/prompt. The AI replaces this with the final content. The hard limits that always apply are already in `AGENTS.md`; here goes the detail specific to this project.

## Instructions for the AI

Deepen the security rules the agent must follow in this repository. Discover the real points **by reading the code** (auth middleware, data-access layer, input validation) — don't invent. Cover what applies, pointing to the file of each mechanism:

- **Authorization**: how the authenticated user/tenant is resolved and how every operation cross-checks the request ID against it. Where the authorization middleware/attribute lives and how to apply it to a new route.
- **Input validation**: library used (zod, FluentValidation, Pydantic...), where to validate, what never to trust coming from the client.
- **Sensitive data**: what not to log, what to mask, where the secrets live (secret manager) and how to read them.
- **Injection**: SQL always parameterized; on the front, escape/sanitize user content before rendering as markup.
- **Multi-tenancy** (if applicable): every query scoped by tenant, derived from the authenticated context — never from a client parameter.

Direct prose, positive language. Show right and wrong side by side only when the mistake is subtle.
