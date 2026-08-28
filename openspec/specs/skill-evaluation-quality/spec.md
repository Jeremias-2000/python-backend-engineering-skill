# Skill Evaluation Quality

## Purpose

Define consistency, compatibility, terminology, authority, and proportionality requirements for evaluating the reusable Python backend Skill.

## Requirements

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

### Requirement: Evaluation detects documentation ambiguity
The evaluation suite SHALL test whether normative examples are copy-safe, whether implicit dependencies are identified, whether normative language is interpreted consistently, and whether domain-logging guidance is applied without duplicate logs.

#### Scenario: Incomplete example is evaluated
- **WHEN** an evaluation presents a reference example with omitted imports or symbols
- **THEN** the agent identifies the missing context or treats the snippet as illustrative instead of copying invalid code

#### Scenario: Recommendation strength is evaluated
- **WHEN** an evaluation presents a recommended tool or pattern in a project with coherent existing alternatives
- **THEN** the agent preserves the existing alternative unless a concrete requirement justifies change

#### Scenario: Domain logging is evaluated
- **WHEN** an evaluation covers an invariant failure and a meaningful state transition
- **THEN** the agent keeps invariant logging at the boundary while allowing direct domain logging only when it adds unique diagnostic value
