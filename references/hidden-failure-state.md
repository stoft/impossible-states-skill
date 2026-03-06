# The hidden failure state

When a value can be **loading**, **missing**, or **failed**, modeling that explicitly in the type avoids overloading `undefined` or a single boolean and makes the UI (or caller) handle every case.

## Problem

- `boolean | undefined` or `hasX: boolean` for “do we have X?” often hides:
  - “not asked yet”
  - “loading”
  - “request failed”
  - “not found”
  - “found”
- Downstream code that only checks “truthy” can mis-handle loading or failure.

## Approach

- Model **all** outcomes as a union (or custom type):
  - e.g. `"loading" | "not-found" | "found" | "failed"` for a single resource.
  - Or a generic pattern: NotAsked | Loading | Failure(error) | Success(data).

## Elm: RemoteData

Common pattern in Elm:

```elm
type RemoteData e a
    = NotAsked
    | Loading
    | Failure e
    | Success a
```

Use the same idea in TypeScript (union of tagged objects) or Gleam (custom type with four constructors) so the type forces handling of loading and failure in the UI or pipeline.

## Benefits

- No guessing what `undefined` or `null` means.
- UI can show loading, error, or empty state explicitly.
- Type checker ensures all cases are handled.

## When to load this reference

- Async data fetching and UI state (loading/error/success).
- “Has template?”, “has user?”, etc. when the answer comes from an endpoint or async operation.
