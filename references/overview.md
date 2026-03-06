# Overview: Making impossible states impossible

## What it means

An **impossible state** is a value that the type system allows but that should never occur in your domain. For example: a cash payment with a card number, or a door that is both open and locked.

- **Goal**: Design types so that invalid or inconsistent combinations cannot be represented.
- **Benefit**: Fewer runtime checks, fewer bugs, and clearer intent. Downstream code cannot “forget” to check a condition when invalid combinations are unrepresentable.

## Core idea

- Prefer **sum types / discriminated unions** so each variant carries only the data that makes sense for that variant.
- Avoid **optional fields** that create invalid combinations (e.g. `method: "cash"` with `cardNumber` present).
- Use **smart constructors** or **opaque/branded types** when values must satisfy invariants (e.g. positive numbers, non-empty lists, intervals with start ≤ end).

## Mental model: symptoms

Common symptoms that suggest impossible states:

1. **Many optional fields** on one type, especially when they are mutually exclusive or mutually dependent.
2. **Boolean flags** that can combine in invalid ways (e.g. open + locked).
3. **“Triple value” booleans** (absent vs false vs true) where meaning is unclear.
4. **Single type with a state field** where only some fields are valid for each state.
5. **Async/loading** represented as `undefined` or a single boolean, hiding “loading” vs “failed”.
6. **Raw primitives** for values that have business rules (e.g. “quantity must be positive”).
7. **Invalid state transitions** (e.g. approving a draft without going through “submitted”) enforced only by runtime checks.

## References

- [Richard Feldman — Making Impossible States Impossible](https://www.youtube.com/watch?v=IcgmSRJHu_8) (talk that popularized the idea).
- Concepts apply across typed languages: Elm, Gleam, TypeScript, F#, etc.
