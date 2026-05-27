<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 02](./README.md)

</div>

---

# Error Handling with Option and Result

> Rust models failure as data, not hidden exceptions.

**You will learn:**
- `Option<T>` for presence/absence
- `Result<T, E>` for success/failure
- `?` operator for concise propagation

**Before this page, you should know:** [Structs, Enums, and Pattern Matching](./05-structs-enums-and-pattern-matching.md)

---

## Option

```rust
fn find_speed(limit: bool) -> Option<u32> {
    if limit { Some(120) } else { None }
}
```

Use `Option` when missing value is expected.

## Result

```rust
fn parse_speed(input: &str) -> Result<u32, std::num::ParseIntError> {
    input.parse::<u32>()
}
```

Use `Result` when operation may fail with a meaningful error.

## Propagate errors with ?

```rust
fn sum_two(a: &str, b: &str) -> Result<u32, std::num::ParseIntError> {
    let x = a.parse::<u32>()?;
    let y = b.parse::<u32>()?;
    Ok(x + y)
}
```

> [!TIP]
> `?` returns early on error and keeps code readable.

---

## Recap

- `Option` handles maybe-present values.
- `Result` handles operations that can fail.
- `?` is the primary Rust pattern for clean error propagation.

## Try it yourself

Write a function that divides two numbers from string input and returns
`Result<f64, String>`, including a custom divide-by-zero error.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Structs, Enums, and Pattern Matching](./05-structs-enums-and-pattern-matching.md) | [Chapter 02](./README.md) · [Rust](../../README.md) · [Home](../../../../README.md) | [Chapter 02 Checkpoint →](./07-chapter-02-checkpoint.md) |

</div>
