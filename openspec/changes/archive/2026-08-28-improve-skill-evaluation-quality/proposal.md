## Why

The Skill is conceptually strong, but its evaluation exposed avoidable ambiguity: metadata claims Python 3.8+ while examples use Python 3.10 syntax, tooling guidance can be read as universally mandatory, and exception and boundary terminology is repeated with slightly different force. These inconsistencies can make agents produce incompatible code, over-engineered projects, or divergent error-handling designs.

## What Changes

- Resolve the declared Python-version policy and align all typed examples with it.
- Distinguish mandatory project requirements from recommendations and justified exceptions.
- Make `ExceptionHandler` guidance use one consistent normative formulation.
- Clarify the transport-neutral exception result, public versus internal fields, retryability, HTTP status ownership, and correlation-ID propagation.
- Clarify `ValueError` usage in domain rules versus structural validation, configuration, and parsing.
- Tighten logging ownership between application layers and the final boundary.
- Define concise terminology for entrypoint, adapter, controller, and handler.
- Reduce duplicated normative guidance by assigning each topic one authoritative reference and using cross-references elsewhere.
- Add missing evaluation coverage for version compatibility, tooling proportionality, terminology, and exception-result boundaries.

## Capabilities

### New Capabilities

- `skill-evaluation-quality`: Define consistency, compatibility, authority, terminology, and evaluation requirements for the reusable Skill.

### Modified Capabilities

- `python-refactoring-guidelines`: Align Python-version compatibility, examples, tooling expectations, and abstraction guidance.
- `skill-guidance-quality`: Extend quality requirements for non-duplicated guidance, exception contracts, terminology, and evaluation coverage.
- `exception-handling`: Clarify the normative handler convention and ownership of result fields, logging, HTTP mapping, retryability, and correlation context.

## Impact

- Active documentation under `python-backend-engineering/`, especially `SKILL.md` and references for refactoring, quality, exceptions, logging, domain, application, and architecture.
- `python-backend-engineering/evals/evals.json` gains targeted scenarios for the identified ambiguity classes.
- Main OpenSpec specs under `openspec/specs/` receive synchronized requirement updates.
- No runtime application, API, database, dependency, or external system behavior changes.
