## Context

The active Skill already separates domain/application exceptions from transport concerns, keeps entrypoints thin, and requires safe logging. It does not yet define a single boundary-handling convention, a stable public error-code model, or equivalent behavior across HTTP, Lambda, workers, and queues. The repository is documentation and evaluation content rather than a running backend, so this change defines guidance for agents without introducing application code.

## Goals / Non-Goals

**Goals:**

- Establish `exceptions.exception_handler` as the canonical location and default import convention for `ExceptionHandler`; deviations require an existing convention or framework constraint and explicit justification.
- Define a transport-neutral classification and mapping core with thin transport adapters.
- Standardize internal categories, stable error codes, a safe public envelope, a transport-neutral result, unknown-error fallback, mapping precedence, chaining, logging ownership, and tests.
- Keep the guidance proportional, non-duplicated, and compatible with existing API contracts.

**Non-Goals:**

- Creating a runtime exception package in this repository.
- Requiring one exception hierarchy, framework, HTTP envelope, or transport implementation in every backend.
- Mapping HTTP status codes into domain or application exceptions.
- Changing existing API, database, retry, or functional behavior without an explicit project requirement.

## Decisions

### Canonical handler convention

Guidance will require the canonical import by default:

```python
from exceptions.exception_handler import ExceptionHandler
```

The handler owns boundary translation, not business rules. Projects may adapt the surrounding package structure only when an existing coherent convention or framework constraint requires it; such deviations must be explicitly justified.

### Transport-neutral core and adapters

`ExceptionHandler` will expose a transport-neutral classification/mapping operation that returns a shared result containing `error_code`, `safe_message`, optional `http_status`, optional `correlation_id`, `retryable`, and internal logging context. HTTP, Lambda, worker, and queue entrypoints will use thin adapters that translate the result into status codes, event responses, retry decisions, or DLQ behavior. Retry and DLQ decisions belong to each transport adapter. A shared HTTP envelope will not be imposed on non-HTTP transports.

### Public error codes and precedence

Internal categories will distinguish domain, application, validation, authentication, authorization, infrastructure, timeout, and unexpected failures. Known externally meaningful failures will use stable `ErrorCode` Enum values. Codes represent public semantics, not HTTP status codes. Mapping will prefer the most specific registered exception/type before base classes and finally the unknown-error fallback; precedence must be deterministic and documented. Domain rules will use named exceptions rather than `ValueError`; `ValueError` is not a permitted domain-rule exception.

### Unknown failures and logging

Unknown exceptions will be handled only at the final boundary, logged once with traceback context (`exc_info=True`) and correlation context, and exposed through a generic internal-error code and safe message. Known expected failures receive contextual logging without a traceback by default. Internal exception text, stack traces, credentials, tokens, and unnecessary personal data will not be returned or logged. Correlation IDs are propagated when available; the guide does not impose a universal generation mechanism.

### Exception translation and chaining

When an adapter or application layer wraps an exception, it will preserve the original cause with `raise ... from error`. Domain and application layers will not depend on HTTP exceptions or transport response types.

### Compatibility and proportionality

Existing mappings and public contracts remain authoritative for existing projects. For new projects, `500` is the only default HTTP status for unexpected failures; other category-to-status mappings remain project-defined. The guide will provide defaults without requiring speculative exception hierarchies, interfaces, or containers. Detailed exception rules will live in one dedicated reference; other references will contain only ownership rules and cross-references.

## Risks / Trade-offs

- [Risk] A canonical import may conflict with an existing project's package layout → [Mitigation] Treat it as the recommended convention and require an explicit documented deviation when an established layout must be preserved.
- [Risk] A universal error envelope can leak HTTP assumptions into other transports → [Mitigation] Share codes and internal results, while adapters own transport-specific output.
- [Risk] Multiple layers may log the same exception → [Mitigation] Assign final-boundary logging to the handler and limit inner layers to contextual wrapping or re-raising.
- [Risk] Overly broad categories can hide actionable distinctions → [Mitigation] Use specific mappings first and reserve the generic fallback for unknown failures.
- [Risk] Documentation duplication can drift → [Mitigation] Make the new exception reference authoritative and update `SKILL.md`, quality guidance, and evals only with concise pointers or testable additions.
