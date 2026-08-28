## Why

The Skill currently distinguishes domain exceptions from transport concerns, but it does not provide a complete, reusable standard for handling, mapping, logging, and exposing failures at application boundaries. This leaves agents to infer conventions and can produce duplicated, transport-specific, or unsafe exception-handling guidance.

## What Changes

- Add a dedicated exception-handling capability for Python backends.
- Establish `exceptions.exception_handler` as the canonical module path for the shared `ExceptionHandler`.
- Define a transport-neutral core with thin adapters for HTTP, Lambda, workers, queues, and other entrypoints.
- Define stable public error codes with an Enum for known error categories or codes.
- Separate internal exception categories from stable public error codes.
- Specify a safe fallback for unknown exceptions using an internal error code and non-sensitive public message.
- Define mapping precedence, exception chaining, logging ownership, correlation context, and prevention of duplicate logs.
- Define a transport-neutral result with error code, safe message, optional HTTP status, optional correlation ID, retryability, and internal logging context.
- Align domain, application, infrastructure, logging, quality, and evaluation guidance without duplicating the normative rules.
- Preserve existing API contracts and avoid functional changes unrelated to exception handling.

## Capabilities

### New Capabilities

- `exception-handling`: Standardize boundary exception handling, error-code mapping, transport adapters, safe responses, logging, chaining, and tests.

### Modified Capabilities

- `skill-guidance-quality`: Extend quality guidance and evaluation scenarios to cover exception mapping, unknown failures, safe logging, chaining, and non-HTTP entrypoints.

## Impact

- `python-backend-engineering/SKILL.md` and relevant reference documents will point to the new exception-handling standard.
- A new reference, expected at `python-backend-engineering/references/python-standard-exceptions.md`, will define the normative guidance.
- Domain, application, infrastructure, logging, and quality references may receive narrowly scoped cross-references or clarifications.
- `python-backend-engineering/evals/evals.json` will gain scenarios for known and unknown exceptions, transport-specific behavior, logging safety, and mapping precedence.
- No application runtime code, public API contract, database behavior, or dependency is changed by this documentation-focused proposal.
- For new projects, provide only an unexpected-error HTTP default of `500`; other HTTP mappings remain project-defined.
