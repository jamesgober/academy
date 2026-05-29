<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 02](./README.md)

---

# Variables, Mutability, and Shadowing

> Rust makes state changes visible so you can trust what a value means at each line.

**You will learn:**
- how `let` creates a binding
- why variables are immutable by default
- when `mut` is the right choice
- how shadowing differs from mutation
- how type inference and annotations work
- how these choices make later ownership rules easier

**Before this page, you should know:** [Chapter 01 - Getting Started](../01-getting-started/README.md)

---

## A Variable Is a Binding

In Rust, `let score = 10;` creates a binding. A binding connects a name
(`score`) to a value (`10`).

```rust
fn main() {
    let score = 10;
    println!("score = {score}");
}
```

Rust can infer the type here. Infer means "figure it out from context." Because
`10` is an integer literal and nothing else constrains it, Rust uses `i32` by
default for many integer situations.

You can also annotate the type:

```rust
fn main() {
    let score: i32 = 10;
    let minutes: u32 = 45;
    let average: f64 = 42.5;

    println!("{score}, {minutes}, {average}");
}
```

Type annotations are useful when:

- the compiler cannot infer the type
- several numeric types are possible
- you want the reader to know the intended meaning

## Immutable by Default

Rust variables cannot be reassigned unless you opt in.

```rust
fn main() {
    let score = 10;
    // score = 20;
    println!("{score}");
}
```

If you uncomment the assignment, Rust reports an error similar to:

```text
error[E0384]: cannot assign twice to immutable variable `score`
```

Beginner translation: "You created `score` as a value that should not be
reassigned. If reassignment is intentional, say so with `mut`."

This default is not Rust being fussy. It protects meaning. When you see a normal
`let`, you know that name will not point to a different value later.

## Use `mut` for Intentional Reassignment

```rust
fn main() {
    let mut score = 10;
    score = 20;

    println!("{score}");
}
```

`mut` means the binding may be reassigned.

Use `mut` when the value changes over time:

```rust
fn main() {
    let mut total = 0;

    for minutes in [30, 45, 25] {
        total += minutes;
    }

    println!("total = {total}");
}
```

Here `mut` is honest. `total` is an accumulator, and accumulators change.

## Mutation Versus Interior Change

This surprises many beginners:

```rust
fn main() {
    let mut topics = Vec::new();
    topics.push(String::from("ownership"));
    topics.push(String::from("borrowing"));

    println!("{topics:?}");
}
```

The binding `topics` is mutable, so you can call methods that change the vector.

Without `mut`:

```rust
fn main() {
    let topics = Vec::new();
    // topics.push(String::from("ownership"));
}
```

Rust rejects the push because the vector would need to change.

## Shadowing Creates a New Binding

Shadowing means declaring a new binding with the same name.

```rust
fn main() {
    let raw_minutes = "45";
    let raw_minutes = raw_minutes.trim();
    let raw_minutes: u32 = raw_minutes.parse().expect("valid number");

    println!("minutes = {raw_minutes}");
}
```

This is not reassignment. It is three different bindings with the same name,
one after another:

```text
raw_minutes: &str  -> "45"
raw_minutes: &str  -> trimmed view
raw_minutes: u32   -> parsed number
```

Shadowing is useful for transformation pipelines where the concept stays the
same but the representation improves.

## When Shadowing Is Better Than `mut`

Use shadowing when each step produces a new meaning or type:

```rust
fn main() {
    let title = "  Rust Ownership  ";
    let title = title.trim();
    let title = title.to_lowercase();

    println!("{title}");
}
```

Use `mut` when one value evolves:

```rust
fn main() {
    let mut total = 0;

    for minutes in [20, 30, 40] {
        total += minutes;
    }

    println!("{total}");
}
```

## Constants

`const` defines a value that must be known at compile time and is always
immutable.

```rust
const MAX_DAILY_MINUTES: u32 = 24 * 60;

fn main() {
    println!("{MAX_DAILY_MINUTES}");
}
```

Use constants for fixed rules, limits, and values that should have one meaning
for the whole program.

## Real Example: Clean and Validate Input

```rust
const MAX_DAILY_MINUTES: u32 = 24 * 60;

fn main() {
    let raw_topic = "  Rust borrowing  ";
    let topic = raw_topic.trim();

    let raw_minutes = "45";
    let minutes: u32 = raw_minutes.parse().expect("minutes should be a number");

    let is_valid = !topic.is_empty() && minutes <= MAX_DAILY_MINUTES;

    if is_valid {
        println!("Study entry: {topic} for {minutes} minutes");
    } else {
        println!("Invalid study entry");
    }
}
```

This example uses:

- immutable bindings for stable values
- shadowing-style cleanup through new names
- a constant for a business rule
- explicit type annotation for parsed minutes

## Reference Links

- [Types, Strings, and Collections](../../reference/types-strings-and-collections.md)
- [Errors, Warnings, and Debugging](../../reference/errors-warnings-and-debugging.md)

---

## Recap

- `let` creates a binding.
- Rust bindings are immutable by default.
- `mut` allows reassignment or mutable method calls.
- Shadowing creates a new binding with the same name.
- Shadowing is excellent for cleanup and type transformation.
- Constants are compile-time fixed values.

## Try It Yourself

Write a program that starts with:

```rust
let raw_minutes = "  90  ";
```

Trim it, parse it into `u32`, compare it to a `MAX_SESSION_MINUTES` constant,
and print whether the session is accepted.

---

[**Next ->** Ownership: The One-Owner Rule](./02-ownership-the-one-owner-rule.md)  
[**<- Previous** Chapter Rust Core Mental Model](./README.md)
