## 1. Create the normative exception guidance

- [x] 1.1 Add `python-standard-exceptions.md` with the canonical `ExceptionHandler` import and boundary-only responsibilities.
- [x] 1.2 Document known exception categories, stable `ErrorCode` Enum semantics, deterministic mapping precedence, and safe unknown-error fallback.
- [x] 1.3 Document transport-neutral results and thin HTTP, Lambda, worker, and queue adapters, including retry and DLQ ownership.
- [x] 1.4 Document logging ownership, correlation context, sensitive-data protection, duplicate-log prevention, and `raise ... from error`.

## 2. Align active Skill references

- [x] 2.1 Add the exception reference to `SKILL.md` and keep its cross-cutting exception rules concise.
- [x] 2.2 Add narrowly scoped cross-references or ownership clarifications to domain, application, infrastructure, and logging references without duplicating normative rules.
- [x] 2.3 Preserve existing API, transport, database, and functional behavior guidance for existing projects.

## 3. Extend quality and evaluation coverage

- [x] 3.1 Add evaluation scenarios for known mappings, specific-over-base precedence, unknown-error safety, and contract preservation.
- [x] 3.2 Add evaluation scenarios for non-HTTP adapters, single logging ownership, sensitive-data protection, and exception chaining.
- [x] 3.3 Review the resulting active references for contradictory or duplicated exception-handling instructions.

## 4. Validate the change

- [x] 4.1 Validate OpenSpec artifacts and confirm every requirement has executable WHEN/THEN scenarios.
- [x] 4.2 Verify documented paths, imports, examples, and evaluation expectations against the repository structure.
