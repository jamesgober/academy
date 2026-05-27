<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 03](./README.md)

</div>

---

# Rust Testing Foundations

> Rust gives built-in testing support through `cargo test` and `#[test]`.

**You will learn:**
- Unit vs integration tests
- Assertion patterns
- How to run and filter tests

**Before this page, you should know:** [Chapter 02 Checkpoint](../02-rust-core-mental-model/07-chapter-02-checkpoint.md)

---

## Unit tests in module

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn adds_values() {
        assert_eq!(add(2, 3), 5);
    }
}
```

## Integration tests

Create `tests/math_tests.rs` at crate root for public API tests.

## Useful commands

```bash
cargo test
cargo test adds_values
cargo test -- --nocapture
```

> [!TIP]
> Keep tests deterministic. Avoid dependence on system time, random seeds, or network by default.

---

## Recap

- Unit tests verify internal behavior.
- Integration tests verify public API behavior.
- `cargo test` is the baseline command in every Rust project.

## Try it yourself

Add one happy-path test and one failure-path test to a parser function.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Chapter Start](./README.md) | [Chapter 03](./README.md) · [Rust](../../README.md) · [Home](../../../../README.md) | [Designing Edge-Case Tests →](./02-designing-edge-case-tests.md) |

</div>
