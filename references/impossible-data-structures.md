# Impossible data structures

Data structures that allow invalid combinations can usually be re-modeled to eliminate those states. A strong signal is **many optional fields** or a single **state/status** field with fields that only apply to some states.

## Rules of thumb

- If only some variants need a field, **split the type into variants** (discriminated union / sum type) so each variant has only the fields that apply.
- **Mutually exclusive** options (e.g. password vs OAuth) → one variant per option, each with its own payload.
- **Mutually inclusive** (e.g. “if enabled then config is required”) → one variant for “off”, one for “on” that carries the config.
- **Triple-value “boolean”** (unknown vs no vs yes) → use an explicit enum/union: e.g. `"pending" | "rejected" | "accepted"`.
- **Illegal combinations** (e.g. open + locked) → replace multiple booleans with a single state type with one value per valid combination.

## Different entity states / sub-types with different field requirements

Common with `state`, `status`, or class hierarchies (vehicles, articles, etc.).

- **Don’t**: One type with `state: "draft" | "published" | "archived"` and optional `publishedAt`, `archivedReason`, etc. That allows e.g. draft with `publishedAt` set.
- **Do**: One variant per state; each variant has only the fields that exist in that state (e.g. Draft has title/body; Published adds publishedAt and comments; Archived has archivedReason and archivedAt).

## Mutually exclusive fields

- **Don’t**: `password?: string; oauthToken?: string` — both can be set or both missing.
- **Do**: `{ method: "password"; password: string } | { method: "oauth"; oauthToken: string }` (or equivalent sum type in Gleam/Elm).

## Mutually inclusive fields

- **Don’t**: `enabled?: boolean; config?: Config` — “enabled” can be true with no config.
- **Do**: `{ enabled: false } | { enabled: true; config: Config }` (or `Disabled | Enabled(Config)` in Gleam/Elm).

## Triple-value boolean

- **Don’t**: `hasAcceptedTerms?: boolean` — `undefined` vs `false` vs `true` is ambiguous.
- **Do**: `termsStatus: "accepted" | "rejected" | "pending"` (or a custom type with three constructors).

## Illegal boolean combinations

- **Don’t**: `isOpen: boolean; isLocked: boolean` — allows open and locked.
- **Do**: Single state type: e.g. `"open" | "closed" | "locked"` or `Open | Closed | Locked`. Same idea applies to other illegal enum combinations (e.g. exchange pair EUR/EUR).

## TypeScript: discriminated union shape

Use a common discriminant (e.g. `type` or `state`) and ensure each branch has only the fields that belong to that branch. Object literals then cannot add wrong fields.

## Gleam/Elm: custom type with one constructor per case

Each constructor carries only the data for that case. No optional fields needed for “sometimes present” data — it’s only present in the constructors where it exists.
