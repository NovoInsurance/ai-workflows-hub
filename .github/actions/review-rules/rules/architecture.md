## Architecture (ARCH) — always in scope

Derived from `clean-code/SKILL.md` §7 and `backend/SKILL.md` §1. When the consumer's
`CLAUDE.md` or a sub-project `development-standard/SKILL.md` documents a different
layering, that document wins and you calibrate these rules to it — but you still
report a verdict for every ID.

### ARCH-01 — Dependency direction is inward
- **Check:** Imports flow UI → BFF/server → API client → upstream, or controller → service → repository. Flag any reverse import (a service importing a controller, a repository importing a service).
- **Severity:** must-fix
- **N-A when:** the documented architecture has no layers, or the diff adds no imports across modules.

### ARCH-02 — No layer skipping
- **Check:** No component reaches past its neighbour — e.g. a controller/handler running a query directly, or a UI component calling the upstream API instead of the project's data layer.
- **Severity:** must-fix
- **N-A when:** no layered code is touched.

### ARCH-03 — No framework or persistence types leaking across boundaries
- **Check:** The HTTP request/response object stops at the controller; ORM entities stop at the repository (mapped to a domain type); framework-specific types do not appear in business-logic signatures.
- **Severity:** must-fix
- **N-A when:** the diff touches only one layer and adds no cross-layer signatures.

### ARCH-04 — No circular imports
- **Check:** The new imports do not create a cycle between modules/packages. A shared piece needed by both sides belongs in a leaf module.
- **Severity:** must-fix
- **N-A when:** no new imports.

### ARCH-05 — Naming and structure match the existing codebase
- **Check:** New files, directories, types, and endpoints follow the conventions already visible in neighbouring code — not a second convention introduced by this PR.
- **Severity:** should-fix
- **N-A when:** no new files or public names.

### ARCH-06 — No path climbing past the package root
- **Check:** No `../..` chains escaping the package/module root where the project provides a workspace alias.
- **Severity:** should-fix
- **N-A when:** the project defines no aliases, or no relative imports are added.

### ARCH-07 — Diff stays scoped to one concern
- **Check:** The diff is feature OR refactor OR docs OR infra. Files unrelated to the stated purpose (drive-by reformatting, unrelated renames, stray config edits) are called out with their paths.
- **Severity:** should-fix
- **N-A when:** never — always report a verdict.

### ARCH-08 — One responsibility per file
- **Check:** No file grows past roughly 300 LOC of unrelated exports, and no new file mixes concerns that the existing structure keeps apart.
- **Severity:** should-fix
- **N-A when:** the diff only edits existing files without growing them materially.

### ARCH-09 — New patterns and public contracts are documented
- **Check:** If the PR introduces a new pattern, a new public API, or a new cross-module contract, the corresponding `CLAUDE.md` / SKILL / README is updated in the same PR.
- **Severity:** should-fix
- **N-A when:** the PR introduces no new pattern or public contract.
