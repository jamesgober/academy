<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 03](./README.md)

</div>

---

# Rust CI Workflows with GitHub Actions

> CI should run formatting, linting, and tests on every pull request.

**You will learn:**
- A practical Rust CI workflow file
- Branch protection integration
- What to test in CI for beginner and intermediate projects

**Before this page, you should know:** [Advanced Testing Options](./03-advanced-testing-options.md)

---

## Baseline workflow

```yaml
name: Rust CI

on:
  pull_request:
  push:
    branches: [main]

jobs:
  checks:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: dtolnay/rust-toolchain@stable
      - uses: Swatinem/rust-cache@v2
      - run: cargo fmt --all -- --check
      - run: cargo clippy --all-targets --all-features -- -D warnings
      - run: cargo test --all-features
```

## What to validate in CI

Minimum:
- formatting consistency
- lint quality gate
- full test pass

For larger repos:
- workspace checks
- doc tests
- feature-flag matrix tests

## Branch protection

Require the CI check before merge on `main`.

> [!IMPORTANT]
> CI is most effective when it is required, not optional.

---

## Recap

- Rust CI should enforce quality gates on pull requests.
- Start with one stable workflow.
- Require CI checks through branch protection.

## Try it yourself

Introduce a formatting violation locally, open a PR, and confirm CI fails then
passes after fixing it.

---

[**Next ->** Debugging Test Failures and Flaky Behavior](./05-debugging-test-failures-and-flaky-behavior.md)  
[**<- Previous** Advanced Testing Options](./03-advanced-testing-options.md)
