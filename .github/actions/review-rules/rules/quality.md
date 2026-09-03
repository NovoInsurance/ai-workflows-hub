## Code Quality (QUAL) — always in scope

Derived from `clean-code/SKILL.md`. Thresholds are the baseline; a sub-project
`development-standard/SKILL.md` may set stricter ones.

### QUAL-01 — Intention-revealing names
- **Check:** No 1–2 letter names outside short loop counters, no noise words (`Manager`, `Helper`, `Util`, `Data`, `Info`, `Wrapper`) where a concrete role exists, no type-suffix doubling (`userList`), booleans read as predicates (`isActive`, `hasPaid`).
- **Severity:** should-fix
- **N-A when:** the diff introduces no new identifiers.

### QUAL-02 — Function length within threshold
- **Check:** No function exceeds ~50 LOC. Pure data-shape mappers are exempt. Report the function name and line range.
- **Severity:** should-fix
- **N-A when:** no functions added or materially grown.

### QUAL-03 — Parameter count within threshold
- **Check:** No more than 4 positional parameters; beyond that the arguments belong in a named options object.
- **Severity:** should-fix
- **N-A when:** no new or changed signatures.

### QUAL-04 — Complexity and nesting within threshold
- **Check:** Cyclomatic complexity ≤ 10 per function and ≤ 3 levels of `if`/`for`/`try` nesting. Deep nesting should become early-return guard clauses.
- **Severity:** should-fix
- **N-A when:** no branching logic added.

### QUAL-05 — No flag arguments, no mutated parameters
- **Check:** No boolean parameter that selects between two behaviours (split the function instead); parameters are treated as immutable rather than mutated in place.
- **Severity:** should-fix
- **N-A when:** no new or changed signatures.

### QUAL-06 — Comments explain why, not what
- **Check:** No line-by-line narration of the code, no commented-out code left behind, no bare `TODO`/`FIXME` without an owner and issue link.
- **Severity:** should-fix
- **N-A when:** the diff adds no comments.

### QUAL-07 — No magic numbers or strings
- **Check:** Values carrying business meaning are named constants, not literals embedded in logic.
- **Severity:** should-fix
- **N-A when:** no literals in business logic.

### QUAL-08 — No debug output in production paths
- **Check:** No `console.*`, `print`, `println`, `dump`, or equivalent left in non-test code — the project logger is used instead.
- **Severity:** must-fix
- **N-A when:** the diff touches only test files or config.

### QUAL-09 — Errors are never swallowed
- **Check:** Every `catch` (or equivalent) logs with context via the project logger, re-throws, or converts to a typed error. No empty catch, no catch that only returns a default silently. `try` scope is narrow rather than wrapping a whole function.
- **Severity:** must-fix
- **N-A when:** the diff adds no error handling.

### QUAL-10 — Error context is sufficient to diagnose
- **Check:** Logged/propagated errors carry enough context to diagnose without reproducing — correlation or request id, key parameters, error code, upstream status.
- **Severity:** should-fix
- **N-A when:** no error paths added.

### QUAL-11 — No floating async work
- **Check:** Every promise/future is awaited, returned, or has an attached error handler. No fire-and-forget async call whose failure disappears.
- **Severity:** must-fix
- **N-A when:** the language or diff has no async code.

### QUAL-12 — Independent async work runs in parallel
- **Check:** No sequential `await` inside a loop over independent items where a bounded fan-out is available.
- **Severity:** should-fix
- **N-A when:** no loops over async work.

### QUAL-13 — Type-system escape hatches are guarded
- **Check:** No `any`, no unchecked casts (`as`, unchecked downcasts) without a runtime guard, no non-null assertions without a control-flow guarantee on the same screen of code. Exported functions have explicit return types.
- **Severity:** should-fix
- **N-A when:** the language is untyped or the diff adds no type annotations.

### QUAL-14 — No duplication past the rule of three
- **Check:** A block copy-pasted a third time is extracted. Do not flag a second occurrence — premature abstraction is also a finding.
- **Severity:** should-fix
- **N-A when:** no repeated blocks introduced.

### QUAL-15 — No dead code
- **Check:** No unreachable branches, unused exports, or code kept "for later" in the diff.
- **Severity:** should-fix
- **N-A when:** nothing removable is present.
