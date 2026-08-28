## 1. Align compatibility and normative language

- [x] 1.1 Resolve the Python 3.8+ policy and update incompatible normative type-annotation examples or add explicit version gates.
- [x] 1.2 Label mandatory requirements, new-project recommendations, and project-defined choices consistently across active references.
- [x] 1.3 Standardize `ExceptionHandler` wording and terminology for entrypoint, adapter, controller, and handler.

## 2. Clarify exception and logging contracts

- [x] 2.1 Document public, transport-specific, adapter metadata, and internal-only fields in the transport-neutral exception result.
- [x] 2.2 Clarify HTTP status ownership, retry/DLQ ownership, correlation-ID propagation, and safe envelope boundaries.
- [x] 2.3 Clarify `ValueError` usage for structural parsing/configuration versus named domain exceptions.
- [x] 2.4 Define logging ownership for known and unexpected failures and restrict direct domain logging.

## 3. Reduce duplication and update documentation

- [x] 3.1 Consolidate detailed exception rules in `python-standard-exceptions.md` and replace competing details with cross-references.
- [x] 3.2 Update `SKILL.md`, README, architecture, quality, refactoring, domain, application, infrastructure, and logging references consistently.
- [x] 3.3 Preserve proportional DDD guidance and avoid introducing mandatory layers, abstractions, dependencies, or tools.

## 4. Extend evaluation coverage

- [x] 4.1 Add evaluation scenarios for Python-version compatibility and version-gated examples.
- [x] 4.2 Add evaluation scenarios for tooling authority, terminology, and proportional architecture.
- [x] 4.3 Add evaluation scenarios for exception-result field ownership, logging policy, and transport adapter responsibilities.

## 5. Validate and synchronize

- [x] 5.1 Validate JSON, Markdown paths, OpenSpec delta requirements, and all requirement scenarios.
- [x] 5.2 Review active references for contradictory normative language and unresolved duplication.
- [x] 5.3 Synchronize modified capabilities into main OpenSpec specs.
