# Invalid state transitions

Workflows (draft → submitted → approved) should not allow invalid transitions. Modeling each **state as its own type** and exposing **transition functions** that take only the correct input state removes the need for runtime “if state !== X throw” checks.

## Problem

- Single type with `state: "draft" | "submitted" | "approved"` and a function `approve(doc: Doc)` forces runtime checks and tests for “can only approve when submitted”.
- Easy to forget a check or to call in the wrong order.

## Approach

- **Separate types per state**: e.g. `Draft`, `Submitted`, `Approved`, each with only the fields that exist in that state.
- **Transition functions** take the exact state they require:
  - `submit(draft: Draft): Submitted`
  - `approve(submitted: Submitted): Approved`
- The type system then makes it impossible to call `approve` with a `Draft`; no validation branch or test needed for that.

## TypeScript

- Define interfaces (or types) for each state with a discriminant, e.g. `state: "draft"` and the right fields.
- Export only the transition functions that take and return the appropriate state types.
- Callers cannot pass a draft into `approve` because the type is wrong.

## Gleam / Elm

- Define a custom type per state (e.g. `Draft(content)`, `Submitted(content, submitted_at)`, `Approved(content, approved_at)`).
- Functions: `submit(draft: Draft, now: String) -> Submitted`, `approve(submitted: Submitted, now: String) -> Approved`.
- Alternative: phantom types so a single `Doc(state)` type is parameterized by state and only certain functions accept certain state parameters; the principle is the same — invalid transitions are unrepresentable.

## Benefits

- No “invalid state” branches or tests for transitions.
- Refactoring is safer: adding a new state or transition forces updates at call sites.
- API documents valid flow by signature.
