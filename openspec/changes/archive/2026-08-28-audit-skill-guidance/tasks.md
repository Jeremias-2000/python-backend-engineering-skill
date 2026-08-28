## 1. Establish Guidance Authority

- [x] 1.1 Define active-document authority and historical treatment for archived OpenSpec material in the Skill documentation.
- [x] 1.2 Consolidate `SKILL.md` to workflow, activation, concise cross-cutting rules, and links; remove redundant topic-specific normative text.
- [x] 1.3 Update references so each topic has one authoritative detailed source and cross-links do not create conflicting requirements.

## 2. Resolve Architecture and Project-Context Ambiguity

- [x] 2.1 Define explicit selection criteria for `ABC` versus `Protocol`, preserving proportional abstraction decisions.
- [x] 2.2 Clarify `app/` and `app/tests/` as new-project targets while preserving coherent existing layouts such as top-level `tests/`.
- [x] 2.3 Make project-configured test tooling authoritative and change universal Pytest language to a recommendation for new projects.
- [x] 2.4 Align `python-standard-refactoring.md`, architecture/application/infrastructure references, quality guidance, and evaluations.

## 3. Correct Operational Guidance and Documentation

- [x] 3.1 Remove stale logging migration references and define one canonical configuration path for each supported runtime.
- [x] 3.2 Replace broad catches and silent logging fallbacks in normative examples with explicit, bounded error handling and preserved log-level configuration.
- [x] 3.3 Clarify async selection using runtime, driver support, and real concurrency criteria, including sequential synchronous Lambda behavior.
- [x] 3.4 Clarify external contract fixture provenance and distinguish versioned upstream samples from hand-authored mocks.
- [x] 3.5 Correct README local installation paths and verify every documented local reference exists.

## 4. Validate Documentation Consistency

- [x] 4.1 Search active Skill files for stale paths, conflicting requirements, duplicate normative rules, and unsupported implementation assumptions.
- [x] 4.2 Validate evaluation scenarios against active normative guidance and confirm archived files are excluded from active instruction loading.
- [x] 4.3 Run existing repository and OpenSpec validation without adding new tooling.
- [x] 4.4 Confirm no application code, runtime dependency, API contract, database schema, or migration behavior changes.
