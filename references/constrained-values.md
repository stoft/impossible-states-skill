# Constrained values

Values that must satisfy invariants (e.g. non-empty list, positive number, start ≤ end) are often outside what the type system can express by itself. Use **smart constructors** and/or **opaque/branded types** so invalid values cannot be constructed by accident.

## Collections with mandatory values (non-empty)

- **Don’t**: Type alias like `NonEmptyArray<T> = T[]` — still allows empty array.
- **Do**: Model as “first + rest” (e.g. `{ first: T; rest: T[] }` in TypeScript, or `NonEmptyList(first, rest)` in Gleam). Provide a single constructor (e.g. `newNonEmpty(first, rest)`) and use it everywhere. In Gleam/Elm, an opaque type prevents constructing invalid values from outside the module.

## Context dependency (e.g. child–parent)

- **Don’t**: Child with optional `parentId` — a child can exist without a parent reference.
- **Do**: Child type requires `parent: Parent` (or equivalent). Only way to create a child is via a function that takes a parent and value; that guarantees the relationship.

## Single constrained value (e.g. positive number)

- **Don’t**: `quantity: number` — can be zero or negative.
- **Do**: Opaque type (Gleam/Elm) or branded type (TypeScript) plus a smart constructor that validates and returns `Result/Option` or throws. All public APIs take the constrained type; no way to get it except through the constructor.

## Mutually constrained values (e.g. interval)

- **Don’t**: `start: Date; end: Date` — nothing enforces start ≤ end.
- **Do**: Same idea: opaque/branded type + single smart constructor that checks `end >= start` and returns the type only on success. Callers cannot build an invalid interval.

## Summary

- **Smart constructor**: Single function that validates and returns the type (or Result/Error). No public way to construct the value otherwise.
- **Opaque type (Gleam/Elm)**: Module exports the type and constructor(s) but not the internal representation; callers cannot bypass validation.
- **Branded type (TypeScript)**: Intersection with `{ __brand: "Name" }` (or `unique symbol`) so only your module can produce the brand; construction goes through a function that validates.
