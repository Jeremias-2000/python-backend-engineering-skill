# Skill Guidance Quality

## Purpose

Define quality requirements for active Python backend Skill guidance and its supporting references.

## Requirements

### Requirement: Active guidance authority
Active Skill files SHALL identify which document is authoritative for each topic, and archived OpenSpec artifacts SHALL be treated as historical rather than executable guidance.

#### Scenario: Agent loads Skill guidance
- **WHEN** an agent reads the Python backend Skill
- **THEN** it can distinguish active normative documents from archived planning history

### Requirement: Non-duplicated normative guidance
The Skill SHALL keep workflow and concise cross-cutting rules in `SKILL.md`, while each reference SHALL own detailed rules for its topic and SHALL avoid conflicting duplicate requirements. Exception classification, mapping, fallback, logging ownership, chaining, and transport adapters SHALL have one detailed authority in `python-standard-exceptions.md`; other references SHALL use concise ownership statements and cross-references.

#### Scenario: Rule is updated
- **WHEN** a topic-specific requirement changes
- **THEN** the agent can identify one authoritative active location to update

#### Scenario: Exception rule is updated
- **WHEN** exception mapping or result behavior changes
- **THEN** the agent updates `python-standard-exceptions.md` and does not create competing detailed mappings in other references

### Requirement: Abstraction selection
The guidance SHALL distinguish nominal class contracts using `ABC` and `abstractmethod` from structural ports using `Protocol`, and SHALL require a concrete architectural justification before introducing either.

#### Scenario: Structural port is sufficient
- **WHEN** a port needs only structural typing and no shared implementation or nominal inheritance
- **THEN** the guidance permits `Protocol` without requiring an `ABC`

#### Scenario: Nominal contract is required
- **WHEN** explicit inheritance or shared abstract behavior is part of the boundary
- **THEN** the guidance permits `ABC` with `abstractmethod`

### Requirement: Context-preserving project layout
For new projects, the guidance SHALL recommend `app/` for application code and `app/tests/` for tests. For existing projects, it SHALL preserve an equivalent coherent layout such as top-level `tests/` unless migration provides a concrete benefit.

#### Scenario: Existing project uses top-level tests
- **WHEN** an existing project has coherent production and top-level test directories
- **THEN** the agent preserves them unless a requested change justifies migration

### Requirement: Existing tooling authority
The guidance SHALL use the project's configured test, type-check, lint, migration, and validation tools. It MAY recommend Pytest for new projects but SHALL NOT require Pytest universally. Recommendations for new-project tooling SHALL be labeled as recommendations and SHALL NOT force replacement of coherent existing tools.

#### Scenario: Project uses a different test runner
- **WHEN** an existing project uses a configured runner other than Pytest
- **THEN** the agent uses that runner and does not add Pytest solely because of the Skill

#### Scenario: Small project lacks optional tooling
- **WHEN** a small script or prototype has no configured lint or type-check tool
- **THEN** the agent does not add tooling unless the task or project policy requires it

### Requirement: Runtime-specific operational examples
Logging and async guidance SHALL provide one canonical configuration path per runtime, avoid stale implementation references, avoid broad catches and silent fallbacks in normative examples, and decide async usage using runtime, driver, and real concurrency criteria.

#### Scenario: Sequential synchronous Lambda flow
- **WHEN** a Lambda invocation performs sequential I/O with a synchronous supported driver
- **THEN** the guidance keeps the flow synchronous unless concurrency or another concrete requirement justifies async

### Requirement: Evidence-based contract fixtures
The guidance SHALL distinguish versioned real external samples used as canonical contract fixtures from hand-authored mocks and SHALL label each accordingly.

#### Scenario: External payload contract is tested
- **WHEN** a test validates an external event or protocol contract
- **THEN** the fixture's provenance is clear and a hand-authored mock is not presented as an upstream sample

### Requirement: Verifiable documentation references
Documented local paths, links, and referenced files SHALL match the repository structure, evaluations SHALL align with active normative guidance, and normative examples SHALL respect declared compatibility or identify version gates.

#### Scenario: Local Skill installation is documented
- **WHEN** a user follows the local installation command
- **THEN** the referenced path exists from the documented working directory

#### Scenario: Documentation example is checked
- **WHEN** an example uses syntax newer than the declared compatibility range
- **THEN** the documentation identifies the required version or provides a compatible alternative

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
- **WHEN** a domain state transition provides diagnostic value unavailable through boundary logging or a domain event
- **THEN** direct domain logging MAY be used without importing framework-specific observability code
