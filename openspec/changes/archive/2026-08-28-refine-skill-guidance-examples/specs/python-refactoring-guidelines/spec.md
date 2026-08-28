## MODIFIED Requirements

### Requirement: Unit-testable design
Refactoring guidance SHALL design boundaries so unit tests can replace persistence, network, queues, clocks, and other external concerns with mocks, stubs, or fakes. Examples SHALL make required dependencies and return behavior clear, and SHALL use integration or contract tests when behavior depends on a real database, framework, protocol, or external service.

#### Scenario: Boundary is refactored
- **WHEN** a repository, service, controller, handler, or equivalent boundary is moved
- **THEN** the example and tests show explicit constructor dependencies and a replacement test double without relying on undefined symbols

### Requirement: Compatibility requirements
Refactoring guidance SHALL preserve existing behavior and SHALL keep normative examples compatible with the declared Python version, or explicitly identify a newer version requirement at the example boundary.

#### Scenario: Python 3.8 example is copied
- **WHEN** an agent copies a normative refactoring example
- **THEN** imports, symbols, return behavior, and type annotations are valid for Python 3.8+ or the example clearly states its version gate
