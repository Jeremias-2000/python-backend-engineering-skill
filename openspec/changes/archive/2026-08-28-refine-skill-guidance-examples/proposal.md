## Why

The Skill is architecturally mature, but a final review found small documentation ambiguities that can reduce implementation reliability: some examples are not fully self-contained, normative strength is not always obvious at the reference level, and the domain-logging exception is described in more than one way. Refining these areas now will make guidance easier to copy safely and reduce divergent interpretations without changing runtime behavior.

## What Changes

- Complete or annotate normative code examples so imports, symbols, return values, and prerequisites are clear.
- Make mandatory rules, recommendations, and project-defined choices explicit throughout active references.
- Consolidate domain-logging guidance into one consistent policy: boundary logging or domain events are preferred; direct domain logging is exceptional and limited to meaningful state transitions.
- Add evaluation scenarios for copyable examples, implicit dependencies, normative-language interpretation, and logging-policy consistency.
- Preserve proportional DDD, Python 3.8+ compatibility, existing tooling, and the single authoritative exception-handling reference.
- Keep operational gaps such as cancellation semantics, metrics, `ErrorCode` versioning, and handler self-failure out of scope for a future change.

## Capabilities

### New Capabilities

### Modified Capabilities

- `skill-guidance-quality`: clarify completeness, normative labeling, and non-duplicated guidance requirements.
- `skill-evaluation-quality`: extend evaluation coverage for self-contained examples and remaining documentation ambiguity.
- `python-refactoring-guidelines`: require copyable refactoring examples to preserve declared compatibility and explicit dependencies.

## Impact

- Active Markdown references under `python-backend-engineering/`.
- `python-backend-engineering/SKILL.md`, README, and evaluation scenarios.
- Main OpenSpec specifications for guidance quality, evaluation quality, and refactoring guidelines.
- No runtime code, API, database, dependency, or infrastructure behavior changes.
