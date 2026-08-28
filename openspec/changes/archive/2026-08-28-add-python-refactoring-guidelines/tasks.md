## 1. Add Refactoring Reference

- [x] 1.1 Create `python-backend-engineering/references/python-standard-refactoring.md` with proportional class-boundary, abstraction, constructor-injection, Punq, test-double, compatibility, Alembic, and verification requirements.
- [x] 1.2 Include a valid `ABC` and `abstractmethod` example covering an abstract contract and concrete implementation.
- [x] 1.3 Ensure reference language distinguishes mandatory constructor injection and Service Locator prohibition from conditional Punq adoption.

## 2. Update Skill Definitions

- [x] 2.1 Update `python-backend-engineering/SKILL.md` to describe the package as a reusable Skill and link the new reference.
- [x] 2.2 Confirm no maintained `.github/skills/`, `.agents/skills/`, or `.claude/` copy of `python-backend-engineering` exists and therefore requires synchronization.
- [x] 2.3 Remove or correct every stale reference to the missing `python-standard-refactoring.md` path.

## 3. Preserve Architectural Contracts

- [x] 3.1 Verify guidance targets `app/` and `app/tests/` while allowing equivalent existing layouts without unnecessary moves.
- [x] 3.2 Verify abstractions are required only for justified replaceable boundaries and use `ABC` with `abstractmethod`.
- [x] 3.3 Verify Punq registration is required when Punq is used or explicitly adopted, with resolution confined to the composition root.
- [x] 3.4 Verify API, database, transaction, error, event, retry, timeout, idempotency, logging, and sync/async behavior preservation is explicit.

## 4. Validate Change

- [x] 4.1 Confirm no maintained mirrored `python-backend-engineering` Skill definitions exist under `.github/skills/`, `.agents/skills/`, or `.claude/`.
- [x] 4.2 Run existing repository validation for Skill structure, references, and OpenSpec artifacts.
- [x] 4.3 Confirm no application code, runtime dependency, API contract, database schema, `CONTEXT.md`, or ADR is changed by this work.
