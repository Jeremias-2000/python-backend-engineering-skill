## ADDED Requirements

### Requirement: Exception guidance has one normative owner
The Skill SHALL designate the dedicated exception-handling reference as the authoritative source for exception classification, mapping, fallback, logging ownership, chaining, and transport adapters, while `SKILL.md` and other references provide only concise boundaries or cross-references.

#### Scenario: Agent needs exception guidance
- **WHEN** an agent decides how to map or log an exception
- **THEN** it can locate one authoritative active reference without conflicting duplicated rules

### Requirement: Exception behavior is evaluated
The evaluation set SHALL include scenarios for known mappings, specific-over-base precedence, named domain exceptions instead of `ValueError`, unknown-error safety, the shared result fields, project-defined HTTP mappings, non-HTTP adapters, single logging ownership, cause preservation, correlation propagation, and preservation of existing contracts.

#### Scenario: Skill guidance is evaluated
- **WHEN** an evaluation exercises exception handling
- **THEN** the expected behavior covers both safe external output and correct internal boundary responsibilities
