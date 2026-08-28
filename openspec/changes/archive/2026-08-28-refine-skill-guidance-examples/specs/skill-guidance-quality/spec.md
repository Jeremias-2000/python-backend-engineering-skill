## MODIFIED Requirements

### Requirement: Active Skill guidance authority
The active Skill files SHALL identify which document is authoritative for each topic, and archived OpenSpec artifacts SHALL be treated as historical rather than executable guidance.

#### Scenario: Agent loads Skill guidance
- **WHEN** an agent reads the Python backend Skill
- **THEN** it can distinguish active normative documents from archived planning history and can locate the detailed authority for each topic

### Requirement: Non-duplicated normative guidance
The Skill SHALL keep workflow and concise cross-cutting rules in `SKILL.md`, while each reference SHALL own detailed rules for its topic and SHALL avoid conflicting duplicate requirements. Exception classification, mapping, fallback, logging ownership, chaining, and transport adapters SHALL have one detailed authority in `python-standard-exceptions.md`; other references SHALL use concise ownership statements and cross-references.

#### Scenario: Rule is updated
- **WHEN** a topic-specific requirement changes
- **THEN** the agent can identify one authoritative active location to update without reconciling conflicting detailed copies

### Requirement: Normative examples are copy-safe
Normative code examples SHALL include the imports, symbol definitions, return behavior, and compatibility assumptions needed to avoid misleading copy-paste use, or SHALL be explicitly labeled as abbreviated illustrative pseudocode. Examples SHALL not imply dependencies or mandatory tooling that the surrounding guidance does not require.

#### Scenario: Agent copies a normative example
- **WHEN** an agent uses a code example as implementation guidance
- **THEN** the example's required imports, symbols, return behavior, and Python-version assumptions are clear

#### Scenario: Example is intentionally abbreviated
- **WHEN** a reference omits setup or unrelated implementation detail
- **THEN** the snippet is labeled as illustrative and does not present omitted symbols as a complete implementation

### Requirement: Domain logging policy is consistent
The guidance SHALL prefer boundary logging or domain events, SHALL permit direct domain logging only for meaningful state transitions when those alternatives cannot provide equivalent context, and SHALL keep invariant-violation logging at the boundary by default.

#### Scenario: Domain invariant fails
- **WHEN** a domain invariant raises a named exception
- **THEN** the domain raises the exception without duplicate logging and the boundary owns the final error log

#### Scenario: Meaningful domain transition occurs
- **WHEN** a domain state transition itself provides diagnostic value unavailable through boundary logging or a domain event
- **THEN** direct domain logging MAY be used without importing framework-specific observability code
