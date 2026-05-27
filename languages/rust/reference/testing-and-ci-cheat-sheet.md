<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../README.md) / [Rust](../README.md) / [Reference](./README.md)

---

# Testing and CI Cheat Sheet

> Daily quality commands and CI essentials for Rust repositories.

## Local command loop

```bash
cargo fmt --all -- --check
cargo clippy --all-targets --all-features -- -D warnings
cargo test --all-features
```

## Targeted test runs

```bash
cargo test test_name
cargo test -- --nocapture
cargo test module_name::
```

## CI baseline checks

- format check
- clippy lint gate
- full test suite

## Pull request checklist

- local checks pass
- CI passes
- edge-case tests included for changed logic
- changelog/version impact evaluated

---

[Reference Index](./README.md) / [Rust](../README.md) / [Home](../../../README.md)