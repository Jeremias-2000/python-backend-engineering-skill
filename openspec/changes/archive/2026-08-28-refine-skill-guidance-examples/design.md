## Context

The active Skill already defines proportional architecture, Python 3.8+ compatibility, exception ownership, and evaluation coverage. The remaining quality issues are documentary: several examples depend on surrounding context, normative strength varies between references, and logging guidance can appear contradictory when read without its qualification.

## Goals / Non-Goals

**Goals:**

- Make normative examples safe to copy or clearly label them as illustrative pseudocode.
- Make requirement strength and project-specific exceptions explicit.
- Establish one concise, consistent policy for direct domain logging.
- Extend evaluations without adding runtime behavior.

**Non-Goals:**

- No runtime code, dependencies, API, database, migration, or tooling changes.
- No new DDD layers or abstractions.
- No expansion into cancellation, metrics, error-code versioning, or handler self-failure.

## Decisions

### Example completeness

Review active Markdown code blocks. Add imports, definitions, return values, or comments only where omission could cause an agent to copy invalid or misleading code. Mark intentionally abbreviated snippets as illustrative rather than making every example a runnable sample.

Alternative rejected: rewriting every example as a standalone project, because that would add noise and obscure the architectural rule.

### Normative language

Use the existing legend consistently: `MUST`/`SHALL` for boundaries, `SHOULD`/`RECOMMENDED` for defaults, and `MAY` for project-defined choices. Preserve project tooling authority and existing compatibility behavior.

Alternative rejected: strengthening all guidance to mandatory requirements, because it would conflict with proportional architecture and existing projects.

### Logging policy

Keep final-boundary logging as the default owner. Permit direct domain logging only for meaningful state transitions when boundary logging or domain events cannot provide equivalent information. Invariant violations remain exceptions handled at the boundary.

Alternative rejected: banning all domain logging, because some domain transitions are useful observability without framework coupling.

### Evaluation strategy

Add focused scenarios that detect incomplete imports/symbols, accidental mandatory recommendations, and inconsistent logging interpretation. Keep existing scenarios unchanged unless their wording conflicts with the refined policy.

## Risks / Trade-offs

- [Risk] More complete examples increase document length → Mitigation: change only examples whose omissions affect correctness.
- [Risk] Extra normative labels make prose heavier → Mitigation: use the established legend and concise cross-references.
- [Risk] Domain logging remains nuanced → Mitigation: state the default, exception, and prohibited case together.

## Migration Plan

1. Update active references, `SKILL.md`, README, and evals.
2. Add delta specs and synchronize main specs after implementation.
3. Validate Markdown paths, JSON, and OpenSpec requirements.
4. Rollback requires reverting documentation and spec changes only.

## Open Questions

None.
