## Frontend (FE) — in scope because the diff touches client-side code

Derived from `frontend/SKILL.md`. Read "component" as the framework's unit of UI
and "store" as its state container.

### FE-01 — Components have one responsibility
- **Check:** A component renders layout, owns state, or fetches data — not all three. Data/logic containers are separated from presentational markup.
- **Severity:** should-fix
- **N-A when:** the diff adds no component.

### FE-02 — Props are a typed contract
- **Check:** Every prop is typed, required vs optional is explicit, and the count stays under ~6 before grouping into an object or splitting the component. Prefer composition/slots over accumulating boolean flags.
- **Severity:** should-fix
- **N-A when:** the diff adds no component interface.

### FE-03 — No business logic in markup
- **Check:** Calculations and non-trivial conditionals live in named functions/hooks above the return; the template shows what, not how.
- **Severity:** should-fix
- **N-A when:** the diff adds no markup.

### FE-04 — Stable list keys
- **Check:** List keys are stable domain ids, not array indices, for any list that can reorder or receive insertions.
- **Severity:** must-fix
- **N-A when:** the diff renders no list.

### FE-05 — No duplicated or derivable state
- **Check:** Values that can be derived from existing state/props are computed, not stored. Server data, URL, and local state cannot disagree. State is lifted only as high as it is actually shared.
- **Severity:** must-fix
- **N-A when:** the diff adds no state.

### FE-06 — State updates are immutable
- **Check:** State objects and arrays are replaced, not mutated in place, so change detection fires.
- **Severity:** must-fix
- **N-A when:** the diff adds no state update.

### FE-07 — Async views model all four states
- **Check:** Every async view handles loading, error, empty, and success. No render path assumes the data is present, and no silent blank screen on failure — errors surface a human message and a retry affordance.
- **Severity:** must-fix
- **N-A when:** the diff adds no async view.

### FE-08 — In-flight requests are cancelled
- **Check:** Requests are aborted or ignored on unmount and when inputs change, so state is never set on a gone view. Independent requests run in parallel rather than as a waterfall.
- **Severity:** must-fix
- **N-A when:** the diff adds no data fetching.

### FE-09 — Design tokens instead of magic values
- **Check:** Colors, spacing, font sizes, radii, and z-index come from the theme/token scale. No hardcoded hex or scattered pixel literals, no escalating z-index numbers, no inline styles for themeable values.
- **Severity:** should-fix
- **N-A when:** the diff adds no styling.

### FE-10 — Accessible by construction
- **Check:** Semantic elements before `<div onClick>`; every input has an associated label and icon-only buttons an accessible name; interactive elements are keyboard-operable with a visible focus state; meaningful images have `alt`; state is not conveyed by color alone; dialogs trap and restore focus and close on `Esc`.
- **Severity:** must-fix
- **N-A when:** the diff adds no interactive UI.

### FE-11 — Client-side data handling is safe
- **Check:** No raw HTML injection, no privileged keys or tokens in the bundle, no sensitive data in web storage, user-supplied URLs validated before navigation, and no PII in console or analytics breadcrumbs.
- **Severity:** must-fix
- **N-A when:** the diff handles no external data or navigation.

### FE-12 — Forms behave correctly under failure
- **Check:** Submit is disabled with progress shown to prevent double submission; field-level errors appear next to the field; user input survives a validation failure; client validation is never the only validation.
- **Severity:** should-fix
- **N-A when:** the diff adds no form.

### FE-13 — No obvious performance regression
- **Check:** Heavy non-critical screens are code-split; long lists are virtualized or paginated; context/prop references are stable enough not to cascade re-renders. Do not flag missing memoization without a concrete cost.
- **Severity:** should-fix
- **N-A when:** the diff adds no significant render path.
