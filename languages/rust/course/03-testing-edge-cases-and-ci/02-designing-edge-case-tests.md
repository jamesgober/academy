<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 03](./README.md)

</div>

---

# Designing Edge-Case Tests

> Most production bugs hide in edge cases, not happy paths.

**You will learn:**
- Edge-case categories
- Boundary-value test strategy
- Invariant-based thinking

**Before this page, you should know:** [Rust Testing Foundations](./01-rust-testing-foundations.md)

---

## Edge-case categories

- empty input
- max/min boundary values
- invalid format
- duplicate/ordering assumptions
- overflow/underflow scenarios

## Boundary test example

```rust
fn is_valid_score(score: u32) -> bool {
    score <= 100
}

#[test]
fn boundary_scores() {
    assert!(is_valid_score(0));
    assert!(is_valid_score(100));
    assert!(!is_valid_score(101));
}
```

## Invariant tests

An invariant is a rule that must always hold.

Example:
- "cart total should never be negative"

> [!IMPORTANT]
> Write the invariant as a sentence before writing the test.

---

## Recap

- Edge cases should be explicit test design, not afterthoughts.
- Boundary tests are simple and high-impact.
- Invariants convert design rules into enforceable tests.

## Try it yourself

Take one existing function and write 5 edge-case tests before adding new
features.

---

[**Next ->** Advanced Testing Options](./03-advanced-testing-options.md)  
[**<- Previous** Rust Testing Foundations](./01-rust-testing-foundations.md)
