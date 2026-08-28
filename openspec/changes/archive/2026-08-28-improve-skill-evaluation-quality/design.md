## Context

The Skill's architecture and DDD guidance is strong, but the audit found consistency risks across metadata, references, examples, and evaluation scenarios. The most concrete issue is the mismatch between the declared Python 3.8+ compatibility and examples using Python 3.10 union syntax. Exception guidance also spans several references and needs one clear authority and field-ownership model.

## Goals / Non-Goals

**Goals:**

- Make declared Python compatibility and examples agree.
- Distinguish normative requirements, recommendations, and project-specific choices.
- Establish one authoritative terminology and exception-handling contract.
- Reduce duplicated rules while preserving concise cross-cutting reminders.
- Add evaluations that detect the identified failure modes.

**Non-Goals:**

- Adding runtime code or dependencies.
- Requiring DDD layers, repositories, services, controllers, handlers, or containers for every project.
- Changing existing API, database, transport, retry, or logging behavior.
- Rewriting every existing example when a precise local clarification is sufficient.

## Decisions

### Python compatibility

Retain Python 3.8+ compatibility because the Skill targets existing backends. Replace version-incompatible built-in generic and union syntax in normative examples with `typing` equivalents, while allowing project-specific references to newer syntax when explicitly gated by the project's supported version.

Alternative rejected: raising the minimum to Python 3.10, because that would unnecessarily exclude existing Python 3.8 and 3.9 projects.

### Normative strength

Use consistent language:

- SHALL/MUST for requirements that protect boundaries or compatibility.
- SHOULD/RECOMMENDED for defaults in new projects.
- MAY for project-defined choices.

Tooling remains project-controlled. New projects receive recommended defaults; existing projects preserve configured tools.

### Exception authority and result ownership

`python-standard-exceptions.md` remains the sole detailed authority. The shared result fields are classified explicitly:

- public candidates: `error_code`, `safe_message`;
- transport-specific: optional `http_status`;
- propagated context: optional `correlation_id`;
- adapter metadata: `retryable`;
- internal-only: `internal_log_context`.

Adapters own HTTP mapping, retry, acknowledgement, reprocessing, and DLQ decisions. The handler classifies and maps; it does not impose transport policy.

### Terminology

Use:

- entrypoint: outer boundary that receives external input and returns or acknowledges output;
- adapter: translator between a transport/runtime and application or handler results;
- controller: HTTP-oriented adapter when the framework uses that term;
- handler: runtime entrypoint or exception-mapping component, qualified by context.

Alternative rejected: renaming all existing terms, because framework vocabulary and existing projects may already use them coherently.

### Domain and logging clarity

Named exceptions are required for domain-rule contracts. `ValueError` remains acceptable for structural parsing and local configuration examples, but examples must label that context. Direct domain logging remains exceptional; boundary or event-based observability is preferred.

## Risks / Trade-offs

- [Risk] Replacing typing syntax makes examples more verbose → [Mitigation] Apply compatibility changes only to normative examples and retain concise syntax where version-gated.
- [Risk] Stronger wording could make project-specific choices appear forbidden → [Mitigation] Label defaults, recommendations, and mandatory boundaries consistently.
- [Risk] Centralizing exception rules may make nearby references feel less complete → [Mitigation] Keep short ownership statements and direct links, not duplicated mappings.
- [Risk] Terminology may not match every framework → [Mitigation] Treat controller and handler as contextual terms and preserve coherent existing usage.

## Migration Plan

1. Update active metadata, references, examples, and evaluations.
2. Synchronize affected OpenSpec main specs.
3. Review diffs for contradictory normative language and validate JSON/spec syntax.
4. No runtime migration or rollback is required; revert documentation changes if needed.

## Open Questions

None. The audit recommendations define the intended scope.
