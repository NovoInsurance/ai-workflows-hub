## Security (SEC) — always in scope

Derived from `security/SKILL.md` (OWASP-aligned). Treat all input as hostile.
A finding here is `must-fix` unless stated otherwise.

### SEC-01 — Parameterized queries only
- **Check:** No user-controlled value concatenated or interpolated into SQL/NoSQL. ORMs use bound parameters, not raw interpolation.
- **Severity:** must-fix
- **N-A when:** the diff constructs no queries.

### SEC-02 — No command injection
- **Check:** No user input reaching a shell. Argument arrays or safe APIs are used; where a shell is unavoidable, input is validated against an allow-list.
- **Severity:** must-fix
- **N-A when:** the diff spawns no processes.

### SEC-03 — No raw HTML sinks
- **Check:** Framework auto-escaping is relied on. `innerHTML`, `dangerouslySetInnerHTML`, `v-html`, and equivalents are absent, or the value passes through a vetted sanitizer.
- **Severity:** must-fix
- **N-A when:** the diff renders no markup.

### SEC-04 — No path traversal
- **Check:** User-supplied paths are resolved and validated against an allowed base directory; `..` segments and absolute paths are rejected.
- **Severity:** must-fix
- **N-A when:** the diff performs no filesystem access from user input.

### SEC-05 — No SSRF
- **Check:** Outbound requests do not target arbitrary user-supplied URLs. Hosts and schemes are allow-listed; internal ranges and cloud metadata endpoints are blocked.
- **Severity:** must-fix
- **N-A when:** the diff makes no outbound requests from user input.

### SEC-06 — No unsafe deserialization
- **Check:** Untrusted data is never deserialized into executable types; payloads are validated against a schema first.
- **Severity:** must-fix
- **N-A when:** the diff deserializes nothing untrusted.

### SEC-07 — Validation at every external boundary
- **Check:** Every inbound payload, query param, path param, and header is validated against a schema before use — type, length, range, format, enum membership. Allow-list, not deny-list. Invalid input returns 4xx, not 5xx.
- **Severity:** must-fix
- **N-A when:** the diff adds no external entry point.

### SEC-08 — Authentication uses vetted primitives
- **Check:** No hand-rolled auth or session handling. Passwords hashed with argon2id/bcrypt/scrypt — never MD5/SHA-1, plaintext, or reversible encryption. Auth endpoints are rate-limited and return generic failure messages.
- **Severity:** must-fix
- **N-A when:** the diff touches no authentication.

### SEC-09 — Authorization checked server-side, per object
- **Check:** Every request is authorized on the server, and object ownership is verified on each access (no IDOR). Privilege is derived from the verified identity, never from a client-supplied role or flag.
- **Severity:** must-fix
- **N-A when:** the diff exposes no protected resource.

### SEC-10 — No secrets in code or git
- **Check:** No API keys, passwords, private keys, tokens, or connection strings in the diff — including test fixtures and committed config. Values come from env or a secret manager.
- **Severity:** must-fix
- **N-A when:** never — always report a verdict.

### SEC-11 — No secrets or PII in logs and client bundles
- **Check:** Tokens, passwords, and personal data are redacted before logging, and privileged credentials are not shipped to a client bundle.
- **Severity:** must-fix
- **N-A when:** the diff adds no logging and no client-side config.

### SEC-12 — Standard crypto and secure randomness
- **Check:** Modern algorithms via vetted libraries (AES-GCM, ChaCha20-Poly1305, TLS 1.2+); no hand-rolled crypto. Security-relevant tokens and ids come from a CSPRNG, never `Math.random()` or an unseeded PRNG.
- **Severity:** must-fix
- **N-A when:** the diff contains no crypto or token generation.

### SEC-13 — Errors fail safe and leak nothing
- **Check:** On error the code denies access rather than falling through to an open state, and responses carry no stack traces, SQL, versions, or internal hostnames.
- **Severity:** must-fix
- **N-A when:** the diff adds no error responses.

### SEC-14 — Public endpoint hardening
- **Check:** Where applicable to the diff: request rate and body size are capped; CORS uses an explicit origin allow-list (never `*` with credentials); state-changing form requests are CSRF-protected; uploads validate type and size, store outside the web root, and use server-generated filenames.
- **Severity:** must-fix
- **N-A when:** the diff adds no public endpoint, CORS config, form, or upload path.

### SEC-15 — Dependency changes are deliberate
- **Check:** New dependencies are maintained and justified, the lockfile is committed alongside the manifest, and pinned versions/digests are used for actions and images. No typosquat-looking package names.
- **Severity:** should-fix
- **N-A when:** the diff changes no dependency manifest.
