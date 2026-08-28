# Exception Handling

## Purpose

Provide authoritative guidance for exception classification, mapping, fallback, logging ownership, chaining, and transport adapters.

## Requirements

### Requirement: Canonical exception handler
The guidance SHALL define `ExceptionHandler` in `exceptions.exception_handler` as the canonical boundary component and SHALL show the default import `from exceptions.exception_handler import ExceptionHandler`. A deviation SHALL require an existing project convention or framework constraint and explicit justification. This convention SHALL use the same normative wording in `SKILL.md` and all exception references.

#### Scenario: Agent selects the handler
- **WHEN** an agent implements exception translation for a backend boundary
- **THEN** it uses or explicitly adapts the canonical `ExceptionHandler` convention rather than inventing a competing central handler

### Requirement: Boundary-only translation
The guidance SHALL keep business exceptions independent of HTTP, Lambda event, queue, and framework response types, and SHALL perform transport translation at the boundary.

#### Scenario: Domain raises a business failure
- **WHEN** a domain invariant is violated
- **THEN** the domain raises a domain-level exception and does not construct a transport response

#### Scenario: HTTP entrypoint handles a failure
- **WHEN** an HTTP entrypoint receives an exception from the application layer
- **THEN** its thin adapter uses the handler result to construct the HTTP response

### Requirement: Stable known error codes
The guidance SHALL distinguish internal categories (domain, application, validation, authentication, authorization, infrastructure, timeout, and unexpected) from an Enum of stable public error codes, and SHALL NOT use HTTP status codes as Enum values.

#### Scenario: Known exception is mapped
- **WHEN** a registered domain, application, validation, authorization, infrastructure, timeout, or not-found exception reaches the boundary
- **THEN** the handler returns its documented public error code and safe public message

### Requirement: Named domain exceptions
The guidance SHALL require named exceptions for domain-rule violations and SHALL prohibit using `ValueError` as the exception contract for domain rules.

#### Scenario: Domain invariant fails
- **WHEN** a domain invariant is violated
- **THEN** the domain raises a named domain exception that can be mapped deliberately at the boundary

### Requirement: Deterministic mapping precedence
The guidance SHALL define deterministic precedence that selects the most specific exception mapping before base-class mappings and uses the generic fallback only when no known mapping applies.

#### Scenario: Specific mapping exists
- **WHEN** an exception matches both a specific exception mapping and a broader base-class mapping
- **THEN** the specific mapping is selected

#### Scenario: No mapping exists
- **WHEN** an exception has no registered mapping
- **THEN** the generic unknown-error result is selected

### Requirement: Safe unknown-error fallback
The guidance SHALL map unknown exceptions to a stable internal-error code and generic public message, without exposing exception text, stack traces, or sensitive data.

#### Scenario: Unexpected exception reaches the final boundary
- **WHEN** an unrecognized exception is handled by the final boundary
- **THEN** the client receives a safe generic result and the internal diagnostic remains available only through protected logs

### Requirement: Transport-specific adapters
The guidance SHALL share classification, error codes, and a transport-neutral result containing `error_code`, `safe_message`, optional HTTP status, optional correlation ID, retryability, and internal logging context. `error_code` and `safe_message` MAY be exposed publicly; `http_status` belongs to HTTP adapters, `correlation_id` is propagated when available, `retryable` is adapter metadata, and `internal_log_context` SHALL remain internal. HTTP, Lambda, workers, queues, and DLQ integrations SHALL define their own response, retry, acknowledgement, and reprocessing behavior.

#### Scenario: Worker receives a retryable failure
- **WHEN** a worker adapter handles a mapped infrastructure or timeout failure
- **THEN** it applies the configured worker retry or DLQ policy using result metadata instead of returning an HTTP envelope

#### Scenario: Internal context reaches an adapter
- **WHEN** an adapter serializes a transport-neutral result
- **THEN** it excludes `internal_log_context` from the external response

### Requirement: Single logging ownership
The guidance SHALL assign final-boundary logging of unhandled exceptions to the exception handler, including traceback and correlation context, and SHALL prohibit duplicate logging of the same failure without added diagnostic value. Expected known failures SHALL receive contextual logging without a traceback by default. Inner layers SHALL log only when they add diagnostic context unavailable at the final boundary.

#### Scenario: Unknown failure is logged
- **WHEN** the handler processes an unknown exception
- **THEN** it records one protected traceback-aware log entry and does not log secrets or unnecessary personal data

#### Scenario: Application already added context
- **WHEN** an application layer logs useful context and re-raises an expected known failure
- **THEN** the final boundary does not emit a redundant equivalent log entry

### Requirement: Cause preservation
The guidance SHALL preserve the original cause when translating exceptions by using exception chaining where a new exception is raised.

#### Scenario: Infrastructure error is wrapped
- **WHEN** an infrastructure exception is translated into an application or boundary exception
- **THEN** the original exception remains available as the chained cause for diagnosis

### Requirement: Safe public envelope
The guidance SHALL define a minimal public error envelope containing `code` and `message`, with `correlation_id` when available, and SHALL exclude internal diagnostics.

#### Scenario: Boundary returns a known error
- **WHEN** a mapped exception is converted into an external response
- **THEN** the response uses the minimal safe envelope and does not expose internal exception details

### Requirement: Project-defined HTTP mappings
For new projects, the guidance SHALL provide `500` as the default HTTP status for unexpected failures while leaving other category-to-status mappings project-defined; existing projects SHALL preserve their established mappings.

#### Scenario: New project handles a validation error
- **WHEN** a new project maps a validation exception
- **THEN** it defines the HTTP status according to its own contract rather than inheriting a mandatory Skill-wide status

### Requirement: Correlation context
The guidance SHALL propagate a correlation ID when one is available and SHALL NOT require a universal generation mechanism across runtimes.

#### Scenario: Request already has correlation ID
- **WHEN** an exception is handled with an existing correlation ID
- **THEN** the ID is available in the internal result, logs, and public envelope when the transport exposes one

### Requirement: Contract-preserving adoption
The guidance SHALL preserve existing API and transport contracts in existing projects and SHALL provide defaults for new projects without requiring unrelated functional changes.

#### Scenario: Existing API has an established error shape
- **WHEN** an agent adds or refactors exception handling in an existing backend
- **THEN** it preserves the established public shape unless the requested change explicitly changes that contract
