## MODIFIED Requirements

### Requirement: Abstract class convention
Architecturally justified nominal class abstractions SHALL use `ABC` and `abstractmethod` from Python's `abc` module. Structural ports MAY use `Protocol` when duck typing is sufficient and no nominal inheritance or shared abstract behavior is required. Concrete implementations SHALL implement every abstract method with valid Python syntax and naming. Normative examples SHALL respect the Skill's declared Python compatibility range or explicitly declare a newer version requirement.

#### Scenario: Repository contract is introduced
- **WHEN** a repository abstraction is architecturally justified
- **THEN** it inherits from `ABC`, declares its contract with `@abstractmethod`, and has a concrete implementation that fulfills the contract

#### Scenario: Structural port is introduced
- **WHEN** a port needs only structural typing
- **THEN** the agent may define it with `Protocol` instead of `ABC`

#### Scenario: Compatible type syntax is selected
- **WHEN** a refactoring example targets the declared Python 3.8+ compatibility
- **THEN** its type syntax is valid for Python 3.8 or the example explicitly requires a newer version

### Requirement: Incremental verification
The executing agent SHALL validate refactoring incrementally using existing project tooling and tests at the affected architectural boundaries, and SHALL report unresolved trade-offs or test gaps. Tool recommendations for new projects SHALL NOT be interpreted as mandatory replacements for configured tooling in existing projects.

#### Scenario: Boundary refactoring is complete
- **WHEN** implementation changes are finished
- **THEN** targeted tests and existing type-check, lint, migration, or architecture checks are run where available, without adding unrelated tooling

#### Scenario: Compatibility risk exists
- **WHEN** direct replacement could break existing callers
- **THEN** the agent uses characterization or contract tests and a compatibility adapter or staged migration where appropriate

#### Scenario: Existing tooling is configured
- **WHEN** an existing project uses a configured test or lint runner
- **THEN** the agent uses that runner and does not add a replacement solely because a different tool is recommended for new projects
