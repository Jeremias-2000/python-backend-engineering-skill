## Why

The Python backend Skill contains repeated and sometimes conflicting guidance across its main definition, references, evaluations, and documentation. Ambiguous rules for `ABC` versus `Protocol`, test locations, logging, async decisions, test tooling, and external fixtures can cause agents to invent project details or apply inappropriate architecture.

## What Changes

- Establish active Skill files as the sole normative source and distinguish archived OpenSpec material as historical.
- Consolidate repeated guidance so `SKILL.md` provides workflow and concise non-negotiable rules while references own topic-specific details.
- Clarify `ABC` versus `Protocol` usage and align evaluations with that distinction.
- Define new-project target directories separately from preserved layouts in existing projects.
- Correct the local installation path in README.
- Remove stale logging migration references and define one canonical configuration flow per runtime.
- Remove broad catches and silent fallback behavior from normative logging examples.
- Clarify async decisions using runtime, driver, and real concurrency criteria.
- Make project-configured test tooling authoritative; treat Pytest as a recommendation, not a universal requirement.
- Clarify canonical external contract fixtures and distinguish real fixtures from hand-authored mocks.
- Preserve the Skill's architecture-proportionality and no-unrequested-functional-change principles.

## Capabilities

### New Capabilities

- `skill-guidance-quality`: Defines consistency, authority, evidence, and hallucination-resistance requirements for the Python backend Skill documentation and evaluations.

### Modified Capabilities

- `python-refactoring-guidelines`: Clarify abstraction conventions, active-document authority, and existing-project layout behavior.

## Impact

- `README.md`
- `python-backend-engineering/SKILL.md`
- Python backend reference documents, especially architecture, infrastructure, logging, and quality.
- `python-backend-engineering/evals/evals.json`
- Active and archived OpenSpec documentation as applicable.
- No application code, runtime dependency, API contract, or database schema changes.
