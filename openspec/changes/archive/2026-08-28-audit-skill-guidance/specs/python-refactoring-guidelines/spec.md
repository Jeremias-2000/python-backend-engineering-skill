## MODIFIED Requirements

### Requirement: Target project organization
The refactoring guidance SHALL recommend application code under `app/` and tests under `app/tests/` for new projects, while allowing an equivalent existing project layout, including a top-level `tests/` directory, to remain when moving files would add risk without improving boundaries.

#### Scenario: Existing project already uses target layout
- **WHEN** application and test files are located under `app/` and `app/tests/`
- **THEN** refactoring keeps production code and tests within those directories

#### Scenario: Existing project uses equivalent layout
- **WHEN** a project has a coherent equivalent layout outside `app/` and `app/tests/`
- **THEN** the agent may preserve that layout and SHALL document the equivalence rather than perform an unnecessary move

### Requirement: Abstract class convention
Architecturally justified nominal class abstractions SHALL use `ABC` and `abstractmethod` from Python's `abc` module. Structural ports MAY use `Protocol` when duck typing is sufficient and no nominal inheritance or shared abstract behavior is required. Concrete implementations SHALL implement every abstract method with valid Python syntax and naming.

#### Scenario: Repository contract is introduced
- **WHEN** a repository abstraction requires an explicit nominal class contract
- **THEN** it inherits from `ABC`, declares its contract with `@abstractmethod`, and has a concrete implementation that fulfills the contract

#### Scenario: Structural port is introduced
- **WHEN** a port needs only structural typing
- **THEN** the agent may define it with `Protocol` instead of `ABC`
