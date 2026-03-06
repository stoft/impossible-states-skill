# Do / don't examples

Short pairs for impossible-state patterns.

## Discriminated union vs optional field (payment)

**Don't:**

```typescript
type Payment = { method: "card" | "cash"; cardNumber?: string };
// allows: { method: "cash", cardNumber: "1234" }
```

**Do:**

```typescript
type Payment = { type: "cash" } | { type: "card"; cardNumber: string };
```

---

## Entity states: one type with many optionals

**Don't:**

```typescript
interface Article {
  state: "draft" | "published" | "archived";
  publishedAt?: string;
  archivedReason?: string;
  // ...
}
```

**Do:**

```typescript
type Article =
  | { state: "draft"; title: string; body: string }
  | { state: "published"; title: string; body: string; publishedAt: string; comments: Comment[] }
  | { state: "archived"; archivedReason: string; archivedAt: string };
```

---

## Mutually exclusive auth

**Don't:**

```typescript
interface Auth { password?: string; oauthToken?: string; }
```

**Do:**

```typescript
type Auth = { method: "password"; password: string } | { method: "oauth"; oauthToken: string };
```

---

## Triple-value “boolean” (terms acceptance)

**Don't:**

```typescript
interface User { hasAcceptedTerms?: boolean; }
// undefined vs false vs true is ambiguous
```

**Do:**

```typescript
interface User { termsStatus: "accepted" | "rejected" | "pending"; }
```

---

## Illegal boolean combinations (door)

**Don't:**

```typescript
interface Door { isOpen: boolean; isLocked: boolean; }
```

**Do:**

```typescript
type Door = { state: "open" } | { state: "closed" } | { state: "locked" };
```

---

## Hidden failure state (async template)

**Don't:**

```typescript
const hasTemplate: boolean | undefined;
```

**Do:**

```typescript
const templateState: "loading" | "not-found" | "found" | "failed";
```

---

## Non-empty collection

**Don't:**

```typescript
type NonEmptyArray<T> = T[];  // still allows []
```

**Do:**

```typescript
type NonEmptyArray<T> = { first: T; rest: T[] };
function newNonEmpty<T>(first: T, rest: T[]): NonEmptyArray<T> { return { first, rest }; }
```

---

## Constrained value (positive number)

**Don't:**

```typescript
interface X { quantity: number; }  // can be 0 or negative
```

**Do (branded type + smart constructor):**

```typescript
type Positive = number & { __brand: "Positive" };
function makePositive(n: number): Positive | Error {
  return n > 0 ? (n as Positive) : new Error("Not positive");
}
```

---

## Valid state transitions (document workflow)

**Don't:**

```typescript
function approve(doc: Doc) {
  if (doc.state !== "submitted") throw new Error("Invalid state");
  // ...
}
```

**Do:**

```typescript
export function submit(draft: Draft): Submitted;
export function approve(submitted: Submitted): Approved;
// Only a Submitted can be approved; compiler enforces it.
```

---

## Gleam: custom type per variant

**Don't:**

```gleam
pub type Payment {
  Payment(method: PaymentMethod, card_number: Option(String))
}
```

**Do:**

```gleam
pub type BetterPayment {
  Cash
  Card(card_number: String)
}
```

---

## Gleam: opaque type for constrained value

**Don't:**

```gleam
pub type BadX { BadX(quantity: Int) }
```

**Do:**

```gleam
pub opaque type Positive { Positive(Int) }
pub fn make_positive(n: Int) -> Result(Positive, String) {
  if n > 0 { Ok(Positive(n)) } else { Error("Not positive") }
}
```
