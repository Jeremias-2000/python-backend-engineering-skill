## ADDED Requirements

### Requirement: Ignore local Python artifacts
The root `.gitignore` SHALL ignore Python bytecode, caches, virtual environments, test and type-check caches, coverage output, profiling output, build output, and package metadata.

#### Scenario: Local Python tooling creates artifacts
- **WHEN** a developer runs Python tooling that creates caches, coverage files, virtual environments, builds, or package metadata
- **THEN** those generated artifacts are ignored by Git

### Requirement: Ignore local development metadata
The root `.gitignore` SHALL ignore common IDE metadata, editor swap files, operating-system files, notebook checkpoints, and local log files.

#### Scenario: Developer uses supported desktop tooling
- **WHEN** an IDE, editor, operating system, notebook tool, or local application creates metadata or logs
- **THEN** those local artifacts are ignored without requiring repository-specific configuration

### Requirement: Protect local environment files
The root `.gitignore` SHALL ignore `.env` files and environment-specific variants while explicitly preserving `.env.example`.

#### Scenario: Developer creates local environment configuration
- **WHEN** a developer creates `.env`, `.env.local`, or another `.env.*` file
- **THEN** it is ignored, except `.env.example`, which remains trackable

### Requirement: Preserve repository source content
The root `.gitignore` SHALL NOT broadly ignore documentation, Skill source directories, `.agents/`, `.claude/`, `.github/`, `openspec/`, evaluation data, or `skills-lock.json`.

#### Scenario: Repository source is checked
- **WHEN** Git evaluates project-owned Markdown, JSON, YAML, Skill, OpenSpec, or lockfile content
- **THEN** those files remain eligible for tracking
