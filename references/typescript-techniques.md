# TypeScript techniques for impossible states

## Discriminated unions

- Use a common literal field (e.g. `type`, `state`, `method`) so TypeScript narrows correctly.
- Each branch of the union has only the fields that belong to that branch.
- Prefer `type X = { type: "a"; a: string } | { type: "b"; b: number }` over one interface with optional `a?` and `b?`.

Example:

```typescript
type Payment = { type: "cash" } | { type: "card"; cardNumber: string };
// Object literal may only specify known properties — cardNumber not allowed when type is "cash"
const p: Payment = { type: "cash", cardNumber: "1234" }; // error
```

## Branded types (nominal typing)

- TypeScript structural typing allows any number to be used where a “positive number” is expected. Use a brand to make the type nominal:
  - `type Positive = number & { __brand: "Positive" };`
  - Or `type UserId = string & { readonly brand: unique symbol };`
- Only your module creates values of that type (via a function that validates). Callers cannot cast arbitrary numbers/strings to the branded type without going through your API.

## Referencing existing fields (indexed access)

- Avoid re-stating primitive types for fields that already exist on a domain type (e.g. `userId: string` everywhere).
- Prefer **indexed access types** so refactors are local and types stay in sync:
  - `type UserId = User["id"];`
  - `function loadUser(id: User["id"]) { ... }`
- Combine with branded/opaque wrappers when you need stronger guarantees (e.g. `type UserId = User["id"] & { __brand: "UserId" };`), but still derive from the single source of truth on the `User` type.

## Smart constructors

- Export a function that takes raw inputs, validates, and returns `ResultType` or `Error` (or throws). Do not export a way to construct the value without validation (e.g. for intervals, positive numbers, non-empty arrays built as `{ first, rest }`).

## Optional / undefined

- Avoid `field?: T` when the field is only valid for some variants; use a discriminated union instead so the variant carries the field only when valid.
- For “loading | failed | success”, use an explicit union type instead of `T | undefined`.

## Private data

- `private` in classes is compile-time only; serialization can leak. Use `#field` (ECMAScript private) when you need runtime privacy.
- For IDs and “opaque” handles, branded types are enough to prevent confusion and forgery at the type level.

## External reference

- [TypeScript: Nominal typing (branded types)](https://www.typescriptlang.org/play#example/nominal-typing)
