## Backend (BE) — in scope because the diff touches server-side code

Derived from `backend/SKILL.md`. Read "service" as the unit of business logic and
"repository" as the data-access layer, whatever the framework calls them.

### BE-01 — Transactions wrap multi-write operations
- **Check:** A logical unit that writes two or more rows/tables is atomic. Partial-write windows are a bug, not an edge case.
- **Severity:** must-fix
- **N-A when:** the diff performs at most one write per operation.

### BE-02 — No N+1 queries
- **Check:** No query issued inside a loop over rows. Independent reads are batched, joined, or eager-loaded.
- **Severity:** must-fix
- **N-A when:** the diff adds no queries in iterative code.

### BE-03 — Every query is bounded
- **Check:** List endpoints paginate; no unbounded full-table select that can grow without limit.
- **Severity:** must-fix
- **N-A when:** the diff adds no list query.

### BE-04 — Migrations are versioned and safe to deploy
- **Check:** Schema changes ship as versioned migrations, are reversible, and are backward-compatible (expand/contract) so the running version survives the deploy.
- **Severity:** must-fix
- **N-A when:** the diff contains no schema change.

### BE-05 — Retryable writes are idempotent
- **Check:** Any operation that a client, queue, or upstream may retry (payment, external POST, queue consumer) carries an idempotency key and is safe to execute twice.
- **Severity:** must-fix
- **N-A when:** the diff adds no retryable write.

### BE-06 — Read-modify-write on shared state is guarded
- **Check:** Concurrency is handled by DB constraints, optimistic locking, or row locks — not app-level check-then-act.
- **Severity:** must-fix
- **N-A when:** the diff adds no read-modify-write on shared rows.

### BE-07 — Errors are typed and centrally mapped
- **Check:** Domain/typed errors are thrown and converted to the API error envelope in one place. No ad-hoc status codes scattered through handlers. 4xx for caller mistakes, 5xx for our faults.
- **Severity:** must-fix
- **N-A when:** the diff adds no error mapping or handler.

### BE-08 — Structured logs with a correlation id
- **Check:** Logs use the project logger with fields (not string concatenation) and carry a request/trace id through the call chain. Log level matches severity, and no debug logging is left in hot paths.
- **Severity:** should-fix
- **N-A when:** the diff adds no logging.

### BE-09 — Configuration is validated at startup
- **Check:** Environment values are parsed into a validated config object once, and missing or invalid config crashes at boot rather than on first request. No hardcoded environment-specific values.
- **Severity:** must-fix
- **N-A when:** the diff reads no configuration.

### BE-10 — No environment branching in business logic
- **Check:** Behaviour differences come from injected config or flags, not `if (env === 'prod')` sprinkled through the code.
- **Severity:** should-fix
- **N-A when:** the diff has no environment checks.

### BE-11 — Connections and long-running work are managed
- **Check:** Pooled connections are reused rather than opened per request, and background/long-running work respects cancellation and a maximum runtime.
- **Severity:** should-fix
- **N-A when:** the diff adds no connection handling or background job.
