## 1. Define Ignore Rules

- [x] 1.1 Add grouped cross-platform rules for Python bytecode, virtual environments, caches, coverage, profiling, builds, and package metadata.
- [x] 1.2 Add rules for IDE/editor metadata, operating-system files, notebook checkpoints, and local logs.
- [x] 1.3 Add `.env` and `.env.*` exclusions with an explicit `.env.example` exception.

## 2. Protect Repository Content

- [x] 2.1 Confirm `.agents/`, `.claude/`, `.github/`, `openspec/`, documentation, evaluations, and `skills-lock.json` are not excluded by broad patterns.
- [x] 2.2 Preserve existing `.gitignore` rules and intent when adding the baseline.

## 3. Validate

- [x] 3.1 Check representative generated and local files with Git ignore evaluation.
- [x] 3.2 Check representative repository-owned files remain trackable.
- [x] 3.3 Run existing repository validation and inspect the final diff.
