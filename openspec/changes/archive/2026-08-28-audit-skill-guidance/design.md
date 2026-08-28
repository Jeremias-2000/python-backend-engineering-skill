## Context

The active Python backend Skill spans `README.md`, `python-backend-engineering/SKILL.md`, reference documents, and evaluations. Several rules are repeated across these files, while some examples and requirements disagree. Archived OpenSpec artifacts also contain historical copies of earlier guidance. The goal is to make active instructions coherent and resistant to unsupported assumptions without changing application code.

## Goals / Non-Goals

**Goals:**

- Establish a clear authority model for active Skill documents.
- Keep `SKILL.md` focused on activation, workflow, and concise non-negotiable boundaries.
- Make topic references authoritative for detailed architecture, logging, infrastructure, quality, and refactoring rules.
- Resolve `ABC` versus `Protocol`, test layout, async, test runner, logging, and fixture ambiguities.
- Correct verifiable documentation errors and align evaluations with normative guidance.
- Preserve proportional architecture and existing-project compatibility.

**Non-Goals:**

- Change Python application behavior, APIs, database schemas, migrations, or runtime dependencies.
- Add a new testing, linting, or documentation tool.
- Treat archived OpenSpec content as active guidance or rewrite history.
- Require one architecture style, test runner, dependency container, or filesystem layout for every project.

## Decisions

### Active documents are authoritative

Only current Skill files, active references, README, and evaluations define behavior. Archived OpenSpec files remain historical and are excluded from instruction loading and stale-reference checks unless explicitly auditing history.

Alternative: remove archived artifacts. Rejected because archive history must remain preserved.

### Single authority per topic

Keep `SKILL.md` concise: activation, workflow, cross-cutting boundaries, and links. Put detailed rules in the matching reference. Remove duplicated normative paragraphs where consolidation is practical, retaining cross-links.

Alternative: repeat all rules for discoverability. Rejected because duplicated rules drift and create conflicting instructions.

### ABC and Protocol have distinct roles

Use `ABC` with `abstractmethod` when an explicit nominal class contract or shared implementation behavior is required. Use `Protocol` when structural typing and duck typing provide the needed port. Neither is universal; abstraction remains conditional on a concrete boundary problem.

Alternative: mandate ABC for every abstraction. Rejected because existing application and infrastructure guidance uses Protocol for valid ports.

### Layout and tooling follow project context

For new projects, recommend `app/` and `app/tests/`. For existing projects, preserve coherent equivalents such as top-level `tests/` when moving files adds no value. Use the project's configured test runner and checks; recommend Pytest for new projects without requiring it universally.

Alternative: force `app/tests/` and Pytest. Rejected because it conflicts with existing evaluations and project-specific tooling.

### Runtime-specific operational guidance

Define one canonical logging bootstrap path for each supported runtime and remove stale migration references. Logging examples must avoid broad catches and silent fallbacks. Async guidance starts with runtime, driver support, and real intra-invocation concurrency; synchronous sequential Lambda flows remain synchronous.

Alternative: infer async from any blocking I/O. Rejected because execution context and concurrency determine whether async improves behavior.

### Evidence-based fixtures

When an external contract requires a fixture, identify whether it is a real upstream sample or a hand-authored test payload. Require real, versioned samples for canonical contract fixtures and label mocks as mocks.

## Risks / Trade-offs

- [Consolidation removes useful context] -> Keep concise summaries and links in `SKILL.md`; retain unique rationale in references.
- [Protocol/ABC distinction remains vague] -> Add explicit decision criteria and align examples/evaluations.
- [Archived text is mistaken for active guidance] -> Mark archive as historical in review rules and scope active searches explicitly.
- [Documentation edits break installer usage] -> Verify all documented local paths against the repository tree.
- [Operational examples become less convenient] -> Prefer explicit, bounded error handling and runtime-specific configuration over silent resilience.
