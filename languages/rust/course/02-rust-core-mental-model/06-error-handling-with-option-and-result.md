<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 02](./README.md)

---

# Error Handling with Option and Result

> Rust makes absence and failure visible in the type system so callers cannot accidentally ignore them.

**You will learn:**
- when to use `Option<T>`
- when to use `Result<T, E>`
- how `match`, `if let`, `ok_or`, `map_err`, and `?` work
- why `unwrap` is not normal app error handling
- how to create beginner-friendly custom error enums

**Before this page, you should know:** [Structs, Enums, and Pattern Matching](./05-structs-enums-and-pattern-matching.md)

---

## `Option<T>` Means Maybe

Use `Option` when a value may be absent and that absence is not an error.

```rust
fn first_topic(topics: &[String]) -> Option<&String> {
    topics.first()
}
```

`Option<T>` has two variants:

```rust
enum Option<T> {
    Some(T),
    None,
}
```

Use it:

```rust
let topics = vec![String::from("Rust")];

match first_topic(&topics) {
    Some(topic) => println!("first topic: {topic}"),
    None => println!("no topics yet"),
}
```

## `Result<T, E>` Means Success or Failure

Use `Result` when an operation can fail and the failure matters.

```rust
fn parse_minutes(input: &str) -> Result<u32, std::num::ParseIntError> {
    input.trim().parse::<u32>()
}
```

`Result<T, E>` has two variants:

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

Use it:

```rust
match parse_minutes("45") {
    Ok(minutes) => println!("{minutes} minutes"),
    Err(error) => println!("invalid minutes: {error}"),
}
```

## Absence Versus Failure

| Situation | Type |
|---|---|
| list might not have a first item | `Option<&T>` |
| map might not contain a key | `Option<&V>` |
| parsing text might fail | `Result<T, ParseError>` |
| file reading might fail | `Result<String, io::Error>` |
| validation might reject input | `Result<T, ValidationError>` |

If the caller needs to know why something failed, use `Result`.

## The `?` Operator

`?` means "if this is `Ok`, unwrap the value; if this is `Err`, return the error
from the current function."

```rust
fn parse_pair(left: &str, right: &str) -> Result<(u32, u32), std::num::ParseIntError> {
    let left = left.trim().parse::<u32>()?;
    let right = right.trim().parse::<u32>()?;

    Ok((left, right))
}
```

This is equivalent to writing a `match` for each parse, but much cleaner.

## Convert `Option` to `Result`

Sometimes absence becomes an error at a boundary.

```rust
#[derive(Debug, PartialEq, Eq)]
enum EntryError {
    EmptyTopic,
}

fn normalize_topic(topic: &str) -> Option<String> {
    let clean = topic.trim();

    if clean.is_empty() {
        None
    } else {
        Some(clean.to_owned())
    }
}

fn require_topic(topic: &str) -> Result<String, EntryError> {
    normalize_topic(topic).ok_or(EntryError::EmptyTopic)
}
```

`ok_or` turns `Some(value)` into `Ok(value)` and `None` into `Err(error)`.

## Convert Error Types with `map_err`

```rust
#[derive(Debug, PartialEq, Eq)]
enum EntryError {
    EmptyTopic,
    InvalidMinutes,
}

fn parse_minutes(input: &str) -> Result<u32, EntryError> {
    input
        .trim()
        .parse::<u32>()
        .map_err(|_| EntryError::InvalidMinutes)
}
```

`map_err` changes the error while leaving success alone.

## Avoid `unwrap` in App Flow

`unwrap()` means "give me the success value, or panic if this is failure."

```rust
let minutes = "abc".parse::<u32>().unwrap();
```

That panics. Panics are not user-friendly error handling.

Good uses of `unwrap`:

- quick scratch experiments
- tests where failure should fail the test
- truly impossible cases with strong proof

Normal application code should return or handle errors.

## Real Validator with Custom Errors

```rust
#[derive(Debug, Clone, PartialEq, Eq)]
struct StudyEntry {
    topic: String,
    minutes: u32,
}

#[derive(Debug, Clone, PartialEq, Eq)]
enum EntryError {
    EmptyTopic,
    InvalidMinutes,
}

impl StudyEntry {
    fn new(topic: &str, minutes: &str) -> Result<Self, EntryError> {
        let topic = normalize_topic(topic).ok_or(EntryError::EmptyTopic)?;
        let minutes = minutes
            .trim()
            .parse::<u32>()
            .map_err(|_| EntryError::InvalidMinutes)?;

        if minutes == 0 {
            return Err(EntryError::InvalidMinutes);
        }

        Ok(Self { topic, minutes })
    }
}

fn normalize_topic(topic: &str) -> Option<String> {
    let clean = topic.trim();

    if clean.is_empty() {
        None
    } else {
        Some(clean.to_owned())
    }
}
```

This teaches a professional pattern:

- raw input is borrowed
- clean data is owned by the struct
- errors are named
- `?` keeps the happy path readable

## Display a Friendly Error

```rust
fn error_message(error: &EntryError) -> &'static str {
    match error {
        EntryError::EmptyTopic => "topic cannot be empty",
        EntryError::InvalidMinutes => "minutes must be a positive number",
    }
}
```

Later you can implement `Display`, but a simple formatter function is a good
beginner step.

## Reference Links

- [Errors, Warnings, and Debugging](../../reference/errors-warnings-and-debugging.md)
- [Functions, Methods, Generics, and Traits](../../reference/functions-methods-generics-and-traits.md)
- [Practical Rust Patterns](../../reference/practical-rust-patterns.md)

---

## Recap

- Use `Option` for normal absence.
- Use `Result` for meaningful failure.
- `?` returns early on error.
- `ok_or` converts absence into failure.
- `map_err` converts one error type into another.
- Avoid `unwrap` in normal user-facing application flow.

## Try It Yourself

Write `StudyEntry::new(topic: &str, minutes: &str) -> Result<StudyEntry, EntryError>`.
Reject empty topics, non-numeric minutes, and zero minutes. Then write tests for
one success and three failures.

---

[**Next ->** Chapter 02 Checkpoint](./07-chapter-02-checkpoint.md)  
[**<- Previous** Structs, Enums, and Pattern Matching](./05-structs-enums-and-pattern-matching.md)
