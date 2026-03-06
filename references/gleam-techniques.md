# Gleam techniques for impossible states

## Custom types with one constructor per case

- Prefer one constructor per logical state or option; attach data only to the constructors that need it.
- Avoid a single record type with many `Option(_)` fields — that allows impossible combinations (e.g. `Cash` with `Some(card_number)`).

Example:

```gleam
// ✅ Cash and Card are separate; Card carries card_number
pub type BetterPayment {
  Cash
  Card(card_number: String)
}
```

## Opaque types

- Use `pub opaque type` when the value must be created only through your module (e.g. validated positive numbers, non-empty lists, user IDs).
- Export the type and constructor name; do not export the constructor’s arguments. Provide functions to create (and optionally to read) the value.
- Prevents callers from constructing invalid values or pattern-matching on internals.

Example:

```gleam
pub opaque type Positive { Positive(Int) }
pub fn make_positive(n: Int) -> Result(Positive, String) {
  if n > 0 { Ok(Positive(n)) } else { Error("Not positive") }
}
```

## Phantom types

- For state machines, you can use a type parameter that indicates the current state (e.g. `Doc(State)` where `State` is Draft | Submitted | Approved). Functions like `approve` then accept only `Doc(Submitted)`.
- Alternative: separate types per state (`Draft`, `Submitted`, `Approved`) and transition functions that take one and return the next. Both approaches make invalid transitions unrepresentable.

## Result and Option

- Use `Result(OkValue, Error)` for operations that can fail (e.g. smart constructors). Use `Option(a)` only when “absent” is a valid semantic (e.g. optional config); avoid overloading Option for “loading” or “error”.
- For async/loading/failure, model with a custom type (e.g. NotAsked | Loading | Failure(error) | Success(data)) instead of `Option(Result(...))` if that clarifies the states.

## External references

- [Gleam: Opaque types](https://tour.gleam.run/advanced-features/opaque-types/)
- [Phantom types in Gleam](https://blog.hayleigh.dev/phantom-types-in-gleam)
