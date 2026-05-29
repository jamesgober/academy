<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 02](./README.md)

---

# Chapter 02 Checkpoint

> Prove you can reason with Rust's core model before testing, async, and project architecture.

## Must-Be-Able Checklist

- Explain the difference between copy and move.
- Point to the owner of a `String`.
- Choose `&str`, `String`, `&T`, or `&mut T` for a function parameter.
- Explain why shared and mutable borrows cannot overlap freely.
- Explain what a lifetime annotation says.
- Build structs with private fields and public methods.
- Use enums to represent one-of-many states.
- Use `match` and `if let`.
- Use `Option`, `Result`, `?`, `ok_or`, and `map_err`.
- Avoid `unwrap` in normal app flow.

## Capstone: Garage Intake Module

Build a small module that processes raw car intake records.

Raw input:

```rust
struct IntakeRequest {
    model: String,
    speed_raw: String,
}
```

Clean accepted data:

```rust
struct Car {
    model: String,
    max_speed: u32,
}
```

Status:

```rust
enum IntakeStatus {
    Accepted(Car),
    Rejected { model: String, reason: String },
}
```

Requirements:

- trim model names
- reject empty model names
- parse speed with `Result`
- reject speed `0`
- avoid unnecessary cloning
- format one output line per request

## Suggested Functions

```rust
fn normalize_model(model: &str) -> Option<String>
fn parse_speed(raw: &str) -> Result<u32, IntakeError>
fn process_request(request: IntakeRequest) -> IntakeStatus
fn format_status(status: &IntakeStatus) -> String
```

Create your own `IntakeError` enum.

## Hints

- Use `ok_or` to convert missing model into an error.
- Use `map_err` to convert parse errors.
- `process_request` may take ownership of `IntakeRequest`.
- `format_status` should borrow because it only reads.
- If you clone, write a comment explaining why the second owned value is needed.

## Solution Direction

Your flow should look like this:

```text
raw request
    |
    v
trim and validate model
    |
    v
parse and validate speed
    |
    |-- success -> Accepted(Car)
    `-- failure -> Rejected { model, reason }
```

## Reviewer Checklist

- Can the learner explain where ownership moves?
- Are borrowed parameters used for read-only formatting and validation?
- Are invalid states represented with enums instead of loose booleans?
- Are errors named and understandable?
- Does the code compile without unnecessary `clone()` calls?

---

## Next

Continue to [Chapter 03 - Testing, Edge Cases, and CI](../03-testing-edge-cases-and-ci/README.md).

---

[**Next ->** Testing, Edge Cases, and CI](../03-testing-edge-cases-and-ci/README.md)  
[**<- Previous** Error Handling with Option and Result](./06-error-handling-with-option-and-result.md)
