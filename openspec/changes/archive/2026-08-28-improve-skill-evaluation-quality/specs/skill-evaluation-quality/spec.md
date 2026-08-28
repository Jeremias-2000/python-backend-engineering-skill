## ADDED Requirements

### Requirement: Evaluation covers guidance consistency
The Skill evaluation suite SHALL test compatibility metadata, normative strength, terminology, authority ownership, and proportionality in addition to backend architecture behavior.

#### Scenario: Agent evaluates a project with existing tooling
- **WHEN** the project already has a configured test, lint, or type-check tool
- **THEN** the agent preserves that tooling and does not add alternatives solely because the Skill recommends them

#### Scenario: Agent evaluates a small backend
- **WHEN** a backend has no meaningful domain complexity or substitution boundary
- **THEN** the agent keeps the architecture minimal and does not create DDD layers or speculative abstractions

### Requirement: Examples respect declared compatibility
Normative examples SHALL use syntax supported by the declared Python compatibility range, or SHALL identify the newer version requirement at the example boundary.

#### Scenario: Python 3.8 project uses the Skill
- **WHEN** an agent copies a normative type annotation example
- **THEN** the example remains valid for Python 3.8 or clearly states that a newer project version is required

### Requirement: Terminology is non-overlapping
The active guidance SHALL define entrypoint, adapter, controller, and handler in a way that prevents agents from creating duplicate components for one boundary.

#### Scenario: HTTP boundary is designed
- **WHEN** a framework calls its HTTP boundary a controller
- **THEN** the agent treats the controller as the HTTP adapter or entrypoint unless a separate responsibility is justified
