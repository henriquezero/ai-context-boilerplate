# task-approach.md — How to approach a task (template)

> Template/prompt. The AI replaces this with the final content in the target repository. Part of `ai-context/rules/`.

## Instructions for the AI

Generate the project's behavior-principles section. The recommended pattern covers:

- Break the task into small steps and validate each one.
- For significant changes, describe the plan before implementing.
- Ask when there's ambiguity — don't invent requirements.
- Before creating a new file of a type, open the equivalent canonical file (see `canonical-files.md`) and follow the same pattern.
- Before touching something shared (schema, API contract, central module, design system), check who else consumes it.
- When generating code that violates a convention, update `ai-context/CONVENTIONS.md` in the same PR (feedback loop).

Adapt to the project. Direct prose, positive language.
