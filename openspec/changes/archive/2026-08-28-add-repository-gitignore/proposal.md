## Why

The repository has an empty `.gitignore`, so local Python environments, caches, coverage output, IDE metadata, operating-system files, logs, and environment secrets can be accidentally included in version control. The repository is documentation and configuration only, so it needs a portable defensive baseline without hiding tracked Skill, OpenSpec, evaluation, or lockfile content.

## What Changes

- Add conventional cross-platform Python and developer-tool exclusions.
- Ignore virtual environments, bytecode, caches, test and type-check output, coverage, profiling, build artifacts, and package metadata.
- Ignore local IDE metadata and operating-system files.
- Ignore `.env` files and local secret/configuration variants while preserving `.env.example`.
- Ignore local logs.
- Keep `.agents/`, `.claude/`, `.github/`, `openspec/`, `skills-lock.json`, evaluations, documentation, and Skill references trackable.

## Capabilities

### New Capabilities

- `repository-gitignore`: Defines repository-level exclusions for generated, local, platform-specific, and sensitive files without excluding project source or documentation.

### Modified Capabilities

<!-- No existing OpenSpec capability requirements are changed. -->

## Impact

- Root `.gitignore`.
- Git tracking behavior for local development artifacts and secrets.
- No application code, API, database, runtime dependency, or build behavior changes.
