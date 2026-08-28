## MODIFIED Requirements

### Requirement: Non-duplicated normative guidance
The Skill SHALL keep workflow and concise cross-cutting rules in `SKILL.md`, while each reference SHALL own detailed rules for its topic and SHALL avoid conflicting duplicate requirements. Exception classification, mapping, fallback, logging ownership, chaining, and transport adapters SHALL have one detailed authority in `python-standard-exceptions.md`; other references SHALL use concise ownership statements and cross-references.

#### Scenario: Rule is updated
- **WHEN** a topic-specific requirement changes
- **THEN** the agent can identify one authoritative active location to update

#### Scenario: Exception rule is updated
- **WHEN** exception mapping or result behavior changes
- **THEN** the agent updates `python-standard-exceptions.md` and does not create competing detailed mappings in other references

### Requirement: Existing tooling authority
The guidance SHALL use the project's configured test, type-check, lint, migration, and validation tools. It MAY recommend Pytest for new projects but SHALL NOT require Pytest universally. Recommendations for new-project tooling SHALL be labeled as recommendations and SHALL NOT force replacement of coherent existing tools.

#### Scenario: Project uses a different test runner
- **WHEN** an existing project uses a configured runner other than Pytest
- **THEN** the agent uses that runner and does not add Pytest solely because of the Skill

#### Scenario: Small project lacks optional tooling
- **WHEN** a small script or prototype has no configured lint or type-check tool
- **THEN** the agent does not add tooling unless the task or project policy requires it

### Requirement: Verifiable documentation references
Documented local paths, links, and referenced files SHALL match the repository structure, evaluations SHALL align with active normative guidance, and normative examples SHALL respect declared compatibility or identify version gates.

#### Scenario: Local Skill installation is documented
- **WHEN** a user follows the local installation command
- **THEN** the referenced path exists from the documented working directory

#### Scenario: Documentation example is checked
- **WHEN** an example uses syntax newer than the declared compatibility range
- **THEN** the documentation identifies the required version or provides a compatible alternative
