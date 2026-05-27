<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 03](./README.md)

</div>

---

# Debugging Test Failures and Flaky Behavior

> A failing test is useful only when you can quickly localize and fix the cause.

**You will learn:**
- Repeatable failure diagnosis flow
- Common flaky-test causes
- Stabilization strategies

**Before this page, you should know:** [Rust CI Workflows with GitHub Actions](./04-rust-ci-workflows-with-github-actions.md)

---

## Failure diagnosis flow

1. Reproduce failure locally.
2. Run specific test with output:

```bash
cargo test my_test_name -- --nocapture
```

3. Reduce input to smallest failing case.
4. Verify fix with full suite.

## Common flaky-test sources

- time-dependent assertions
- random values without fixed seed
- shared mutable global state
- tests depending on execution order

## Stabilization techniques

- isolate state per test
- control randomness
- avoid wall-clock assumptions
- use deterministic fixtures

> [!WARNING]
> Retrying flaky tests without root-cause analysis hides quality debt.

---

## Recap

- Local reproducibility is step one.
- Flakiness usually comes from nondeterminism.
- Stable tests are foundational for trustworthy CI.

## Try it yourself

Take one time-dependent test and refactor it to deterministic behavior.

---

[**Next ->** Chapter 03 Capstone](./06-chapter-03-capstone.md)  
[**<- Previous** Rust CI Workflows with GitHub Actions](./04-rust-ci-workflows-with-github-actions.md)
