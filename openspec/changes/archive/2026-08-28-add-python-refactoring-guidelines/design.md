## Context

`python-backend-engineering` is distributed as a reusable Skill with mirrored definitions, not as an autonomous Agent. Its primary `SKILL.md` already promotes proportional architecture and references a Python refactoring standard that is missing from the repository. The change must complete that contract without modifying an application, API, database, or runtime dependency.

The guidance must work for existing Python backends with different layouts and dependency containers. It must target `app/` and `app/tests/` while avoiding unnecessary file moves, use abstractions only where they protect a real boundary, and preserve observable behavior during refactoring.

## Goals / Non-Goals

**Goals:**

- Add a dedicated, authoritative refactoring reference and link it from the primary Skill.
- Clarify Skill-versus-Agent terminology.
- Define conditional, proportional use of classes, abstract contracts, Punq, constructor injection, and test doubles.
- Make compatibility, Alembic, composition-root, and verification requirements explicit.
- Keep mirrored distribution surfaces consistent with the primary definition.

**Non-Goals:**

- Refactor an application or add production Python code.
- Introduce Punq, Alembic, or any other runtime dependency into this repository.
- Require interfaces for every class or force migration from an equivalent existing project layout.
- Change existing API contracts, database behavior, or application functionality.
- Create a project glossary or ADR; this change documents reusable engineering guidance, not domain decisions.

## Decisions

### Use a dedicated reference

Create `python-standard-refactoring.md` under the primary reference directory and update the Skill's existing link to it. A separate reference keeps refactoring rules discoverable without duplicating the broader architecture, domain, application, infrastructure, and quality documents.

Alternative considered: fold the content into the quality or architecture reference. Rejected because refactoring has distinct compatibility and migration concerns and the Skill already names a dedicated document.

### Keep the package modeled as a Skill

Describe the package as a reusable Skill that guides an executing agent. Do not add an Agent implementation or agent runtime configuration to the primary package.

Alternative considered: rename or convert it to an Agent. Rejected because the repository's front matter, activation model, and distribution surfaces already represent reusable instructions.

### Make architecture proportional and Punq conditional

Require `app/` and `app/tests/` as the target layout, but permit an equivalent existing layout when moving files adds risk without improving boundaries. Require abstract contracts only for replaceable or independently testable boundaries. Require Punq registration only when Punq is already used or explicitly adopted, with resolution confined to the composition root.

Alternative considered: mandate all interfaces and Punq for every Python project. Rejected because it conflicts with the existing proportional-architecture rule and creates speculative complexity.

### Preserve behavior by default

Define API contracts, response and error behavior, transaction semantics, database behavior, events, retries, timeouts, idempotency, logging, and sync/async behavior as preserved unless the user explicitly requests a change. Prefer incremental migration and compatibility adapters where direct replacement risks callers.

Alternative considered: allow broad cleanup during refactoring. Rejected because it obscures regressions and violates the stated refactoring-only scope.

### Validate at existing boundaries

Require the executing agent to use the project's existing test, type-check, lint, migration, and architecture checks at a proportional scope, updating tests and composition together with moved boundaries. Do not add new tooling solely for this guidance change.

## Risks / Trade-offs

- [Mirrored copies drift] -> Update the primary Skill and maintained `.github`, `.agents`, and `.claude` copies consistently, then compare them.
- [Guidance creates over-abstraction] -> Repeat the abstraction gate and require a concrete problem, owner, protected boundary, and testability or maintainability benefit.
- [Existing layout is treated as invalid] -> State that `app/` and `app/tests/` are targets, while equivalent layouts can remain when behavior and boundaries are preserved.
- [Conditional Punq guidance is misread as optional constructor injection] -> Keep constructor injection and Service Locator prohibition unconditional; only container adoption and registration are conditional.
- [Refactoring changes behavior] -> Require characterization or contract tests where needed and explicit checks for API, database, transaction, and external-integration behavior.
