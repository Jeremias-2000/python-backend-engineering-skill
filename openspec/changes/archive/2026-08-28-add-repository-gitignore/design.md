## Context

The repository is a documentation and configuration package with an empty root `.gitignore`. It contains Python Skill references, JSON evaluations, OpenSpec specifications and archives, mirrored Skill surfaces, and a lockfile, but no application build or test configuration. The ignore file must prevent accidental tracking of local artifacts without hiding project content.

## Goals / Non-Goals

**Goals:**

- Add a cross-platform baseline for Python and common developer-local artifacts.
- Protect environment files and local secrets.
- Keep Skill sources, documentation, evaluations, OpenSpec content, and `skills-lock.json` trackable.
- Preserve any existing ignore intent; the current file is empty.

**Non-Goals:**

- Ignore repository documentation, `.agents/`, `.claude/`, `.github/`, `openspec/`, or Skill files.
- Add application-specific rules for tools not present in the repository.
- Remove or rewrite existing tracked files.

## Decisions

### Use conventional cross-platform categories

Group rules by Python caches, virtual environments, test/type-check caches, coverage/profiling, build output, notebooks, IDE files, operating-system files, environment secrets, and logs. Use directory patterns with trailing slashes and explicit exceptions where needed.

Alternative: use a minimal Python-only template. Rejected because this repository is edited across multiple operating systems and IDEs.

### Protect environment examples

Ignore `.env` and `.env.*`, then unignore `.env.example`. This prevents local credentials from being tracked while preserving a safe configuration template.

Alternative: ignore only `.env`. Rejected because variants such as `.env.local` and `.env.production` can contain secrets.

### Keep repository-owned content visible

Do not add broad rules for documentation, configuration, Skill distribution directories, OpenSpec, lockfiles, or evaluation data. These are source content, not generated artifacts.

Alternative: ignore all hidden directories. Rejected because it would hide `.github/`, `.agents/`, `.claude/`, and OpenSpec metadata.

## Risks / Trade-offs

- [A future generated directory is not ignored] -> Add a narrowly scoped rule when a real tool is introduced; avoid speculative patterns.
- [A shared IDE setting is ignored] -> Re-include specific team-owned files if they become intentionally versioned.
- [A secret-like file is force-added] -> `.gitignore` reduces accidental tracking but does not replace secret scanning or review.

## Migration Plan

Add rules to the empty root `.gitignore`. No migration or rollback is needed beyond removing the new rules if they cause an unintended match.

## Open Questions

None. Repository inspection shows no existing tool-specific configuration requiring additional rules.
