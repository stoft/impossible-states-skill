# Private data

Data that must not be exposed or confused with raw primitives can be enforced by the type system so callers cannot inspect or forge it.

## Goals

- **Encapsulation**: Callers cannot read internal representation (e.g. user ID as string).
- **Unforgeability**: Values of the type can only be created via your API (e.g. after validation).

## TypeScript

- **Branded types**: `type UserId = string & { readonly brand: unique symbol }`. Only your module creates `UserId` via a function; callers use `UserId` in APIs but cannot create one from an arbitrary string without going through your function. (Runtime is still string; branding is for type safety.)
- **private**: Class fields with `private` are compile-time only; `JSON.stringify` or reflection can still expose them.
- **ECMAScript private (#field)**: Real privacy at runtime; use when you need to prevent serialization/reflection from leaking the value.

## Gleam / Elm

- **Opaque types**: Export the type name and constructor name but not the constructor’s arguments. Other modules can hold and pass the value but cannot pattern-match on it or create it; only your module can create and (if needed) unwrap via a function like `user_id_to_string`.

## When to use

- IDs (userId, orderId) so they are not confused with raw strings and cannot be forged.
- Sensitive data (e.g. SSN) where you want to expose only limited operations (e.g. “last 4 digits”) and prevent accidental leakage.
