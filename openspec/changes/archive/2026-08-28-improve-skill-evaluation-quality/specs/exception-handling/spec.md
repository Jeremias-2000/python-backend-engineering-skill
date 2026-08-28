## MODIFIED Requirements

### Requirement: Canonical exception handler
The guidance SHALL define `ExceptionHandler` in `exceptions.exception_handler` as the canonical boundary component and SHALL show the default import `from exceptions.exception_handler import ExceptionHandler`. A deviation SHALL require an existing project convention or framework constraint and explicit justification. This convention SHALL use the same normative wording in `SKILL.md` and all exception references.

#### Scenario: Agent selects the handler
- **WHEN** an agent implements exception translation for a backend boundary
- **THEN** it uses or explicitly adapts the canonical `ExceptionHandler` convention rather than inventing a competing central handler

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
