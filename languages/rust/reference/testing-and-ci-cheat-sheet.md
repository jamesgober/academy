<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../README.md) / [Rust](../README.md) / [Reference](./README.md)

---

# Testing And CI Reference

> Command lookup for Rust tests, doc tests, targeted runs, strict local checks,
> and CI gates.

Learn the full workflow in [Chapter 03](../course/03-testing-edge-cases-and-ci/README.md).

---

## Local Quality Gate

| Command | Purpose | Notice |
|---|---|---|
| `cargo fmt --all -- --check` | Verify formatting | Use `cargo fmt --all` to fix |
| `cargo check` | Fast compile/type check | Does not run tests |
| `cargo test` | Run unit, integration, and doc tests | Default for local work |
| `cargo test --all-features` | Test with all features enabled | Good CI default for apps |
| `cargo clippy --all-targets --all-features -- -D warnings` | Strict lint gate | Can feel harsh while experimenting |

Recommended before a PR:

```bash
cargo fmt --all -- --check
cargo check
cargo test
cargo clippy --all-targets --all-features -- -D warnings
```

---

## Target Tests

| Goal | Command |
|---|---|
| Run tests whose names contain text | `cargo test parse_entry` |
| Show printed output | `cargo test parse_entry -- --nocapture` |
| Run one integration test file | `cargo test --test public_api` |
| Run doc tests only | `cargo test --doc` |
| Run ignored tests | `cargo test -- --ignored` |
| Run tests single-threaded | `cargo test -- --test-threads=1` |

Use single-threaded runs only for diagnosis. If a test passes only with
`--test-threads=1`, it may be sharing state with another test.

---

## Assertion Macros

| Macro | Use |
|---|---|
| `assert!(condition)` | Boolean condition |
| `assert_eq!(actual, expected)` | Equality |
| `assert_ne!(actual, unexpected)` | Inequality |
| `panic!("message")` | Explicit failure path |
| `matches!(value, pattern)` | Pattern check |

Example:

```rust
assert_eq!(parse_minutes("45"), Ok(45));
assert!(matches!(parse_minutes("bad"), Err(ParseError::InvalidMinutes { .. })));
```

---

## Test Attributes

| Attribute | Use | Example |
|---|---|---|
| `#[test]` | Normal test | `#[test] fn works() {}` |
| `#[should_panic]` | Test expected panic | Prefer `expected = "text"` |
| `#[ignore]` | Skip unless requested | For slow/manual tests |
| `#[cfg(test)]` | Compile only during tests | Unit test modules |

Panic test:

```rust
#[test]
#[should_panic(expected = "max score must be greater than zero")]
fn rejects_zero_max_score() {
    percentage(8, 0);
}
```

Prefer `Result` for normal user-facing errors.

---

## Integration Test Layout

```text
my-crate/
  src/
    lib.rs
  tests/
    common/
      mod.rs
    public_api.rs
```

`tests/public_api.rs`:

```rust
use my_crate::parse_entry;

#[test]
fn parses_public_entry_format() {
    let entry = parse_entry("Rust|45|ownership").unwrap();

    assert_eq!(entry.topic(), "Rust");
}
```

Integration tests use your crate like an outside caller.

---

## GitHub Actions Baseline

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
        with:
          components: rustfmt, clippy
      - uses: Swatinem/rust-cache@v2
      - run: cargo fmt --all -- --check
      - run: cargo clippy --all-targets --all-features -- -D warnings
      - run: cargo test --all-features
      - run: cargo test --doc
```

Tradeoff:

- Simple enough for most learning projects
- Uses common third-party actions for toolchain/cache setup
- For highly locked-down organizations, pin action SHAs and follow internal
  supply-chain policy

---

## Failure Triage

Use this order:

```text
1. Find first failed command.
2. Run that exact command locally.
3. Run only the failing test.
4. Add -- --nocapture if output helps.
5. Shrink the failing input.
6. Fix code or fix the test expectation.
7. Run the full gate again.
```

See [Debugging Test Failures And Flaky Behavior](../course/03-testing-edge-cases-and-ci/05-debugging-test-failures-and-flaky-behavior.md).

---

[Reference Index](./README.md) / [Rust](../README.md) / [Home](../../../README.md)
