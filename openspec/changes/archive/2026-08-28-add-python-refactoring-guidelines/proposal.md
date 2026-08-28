## Why

The `python-backend-engineering` package is a reusable Skill, not an autonomous Agent, but its refactoring guidance is referenced through a missing document. This creates an incomplete contract for Python refactoring and leaves important decisions about class boundaries, Punq, constructor injection, compatibility, and verification implicit.

## What Changes

- Add a dedicated Python refactoring capability reference.
- Define `app/` and `app/tests/` as the target locations for production code and tests, while allowing equivalent existing layouts when migration is unnecessary.
- Establish proportional, class-based boundaries for repositories, services, controllers, handlers, and equivalent replaceable components.
- Require `ABC` and `abstractmethod` for abstractions that are architecturally justified.
- Define constructor dependency injection and prohibit Service Locator usage.
- Document conditional Punq registration for abstractions and concrete implementations, resolved from the composition root.
- Require testable boundaries using mocks, stubs, and fakes.
- Require coordinated updates to imports, composition, Punq configuration, Alembic, and tests when affected.
- Preserve API contracts, database behavior, and existing functional behavior unless explicitly changed.
- Correct the missing reference and clarify that the package is a Skill rather than an Agent.

## Capabilities

### New Capabilities

- `python-refactoring-guidelines`: Defines safe, proportional Python backend refactoring practices, dependency injection boundaries, abstraction conventions, compatibility requirements, and verification expectations.

### Modified Capabilities

<!-- No existing OpenSpec capabilities have requirements requiring modification. -->

## Impact

- `python-backend-engineering/SKILL.md` reference links and refactoring guidance.
- New reference documentation under `python-backend-engineering/references/`.
- Mirrored Skill distributions under `.github/skills/`, `.agents/skills/`, and `.claude/`, if those copies are maintained by the repository workflow.
- No application API, database schema, or runtime dependency changes.
