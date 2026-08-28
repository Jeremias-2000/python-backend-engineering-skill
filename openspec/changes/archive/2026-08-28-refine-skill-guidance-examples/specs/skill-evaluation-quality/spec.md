## ADDED Requirements

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
