## Testing (TEST) — in scope because the diff touches tests, or changes behaviour

Derived from `testing/SKILL.md`. A test exists to let the team change code with
confidence; a flaky or implementation-coupled test is worse than no test.

### TEST-01 — Behavioural change ships with its test
- **Check:** New or changed behaviour is covered in this PR at the cheapest level that gives real confidence. A bug fix includes a regression test that would fail without the fix.
- **Severity:** must-fix
- **N-A when:** the diff changes no behaviour (docs, config, formatting only).

### TEST-02 — Tests are readable and single-purpose
- **Check:** Arrange/Act/Assert are visually separated, each test covers one behaviour, and names describe behaviour (`returns 404 when the resource does not exist`) rather than the method under test.
- **Severity:** should-fix
- **N-A when:** the diff adds no tests.

### TEST-03 — No conditional logic in tests
- **Check:** No `if`/`for`/`try` deciding what to assert. Such a test should be split or parameterized.
- **Severity:** should-fix
- **N-A when:** the diff adds no tests.

### TEST-04 — Assertions target behaviour, not internals
- **Check:** Tests assert on observable outputs and effects, not private state or call counts of internal helpers. A correct refactor must not break them.
- **Severity:** should-fix
- **N-A when:** the diff adds no tests.

### TEST-05 — Edges are covered, not just the happy path
- **Check:** Empty, null/absent, boundary values, and error paths are exercised for the behaviour being added.
- **Severity:** should-fix
- **N-A when:** the diff adds no tests.

### TEST-06 — Only external collaborators are mocked
- **Check:** The system under test is not mocked. Network, DB, clock, randomness, and third-party SDKs are the things doubled, and doubles are reset between tests.
- **Severity:** must-fix
- **N-A when:** the diff adds no test doubles.

### TEST-07 — Tests are deterministic
- **Check:** No dependence on wall-clock time, real network, locale/timezone, or execution order. Time and RNG are injected or frozen. No sleep-based waits — wait on a condition instead.
- **Severity:** must-fix
- **N-A when:** the diff adds no tests.

### TEST-08 — Tests are independent and parallel-safe
- **Check:** No shared mutable state between tests; fixtures reset per test; each integration test sets up and tears down its own data.
- **Severity:** must-fix
- **N-A when:** the diff adds no tests.

### TEST-09 — Test data is built, not copy-pasted
- **Check:** Domain objects come from factories/builders with per-test overrides, tests set only the fields relevant to the behaviour, and no production data or real PII appears in fixtures.
- **Severity:** should-fix
- **N-A when:** the diff adds no fixtures.

### TEST-10 — No assertion-free tests
- **Check:** Every test asserts an outcome. A test that only executes code to raise coverage is a finding.
- **Severity:** should-fix
- **N-A when:** the diff adds no tests.
