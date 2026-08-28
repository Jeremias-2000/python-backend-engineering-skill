## ADDED Requirements

### Requirement: Active guidance authority
Active Skill files SHALL identify which document is authoritative for each topic, and archived OpenSpec artifacts SHALL be treated as historical rather than executable guidance.

#### Scenario: Agent loads Skill guidance
- **WHEN** an agent reads the Python backend Skill
- **THEN** it can distinguish active normative documents from archived planning history

### Requirement: Non-duplicated normative guidance
The Skill SHALL keep workflow and concise cross-cutting rules in `SKILL.md`, while each reference SHALL own detailed rules for its topic and SHALL avoid conflicting duplicate requirements.

#### Scenario: Rule is updated
- **WHEN** a topic-specific requirement changes
- **THEN** the agent can identify one authoritative active location to update

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
The guidance SHALL use the project's configured test, type-check, lint, migration, and validation tools. It MAY recommend Pytest for new projects but SHALL NOT require Pytest universally.

#### Scenario: Project uses a different test runner
- **WHEN** an existing project uses a configured runner other than Pytest
- **THEN** the agent uses that runner and does not add Pytest solely because of the Skill

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
Documented local paths, links, and referenced files SHALL match the repository structure, and evaluations SHALL align with active normative guidance.

#### Scenario: Local Skill installation is documented
- **WHEN** a user follows the local installation command
- **THEN** the referenced path exists from the documented working directory
