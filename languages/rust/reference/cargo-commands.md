<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../README.md) / [Rust](../README.md) / [Reference](./README.md)

---

# Cargo Commands Reference

> Quick lookup. For full context, use [Chapter 01](../course/01-getting-started/).

## At a glance

| Command | Purpose | Typical use |
|---------|---------|-------------|
| `cargo new <name>` | Create project | Start new binary crate |
| `cargo init` | Create project in current folder | Add Cargo to an existing folder |
| `cargo run` | Build and run | Local dev loop |
| `cargo build` | Build without running | Produce debug artifact |
| `cargo build --release` | Optimized build | Performance/release checks |
| `cargo check` | Fast compile check | Frequent feedback |
| `cargo test` | Run tests | Validate behavior |
| `cargo fmt` | Format code | Style consistency |
| `cargo clippy` | Lint code | Catch common issues |
| `cargo doc --open` | Build/open docs | Inspect API docs |
| `cargo add <crate>` | Add dependency | Update `Cargo.toml` safely |
| `cargo update` | Update lockfile versions | Refresh compatible dependencies |
| `cargo clean` | Delete build artifacts | Troubleshoot stale build state |

---

## Common command forms

```bash
cargo run
cargo check
cargo test
cargo test test_name
cargo add serde
cargo run -p cli
cargo run --example print_report
cargo doc --open
cargo clean
```

> [!TIP]
> Use `cargo check` more often than `cargo build` while iterating. It is usually faster.

> [!WARNING]
> `cargo clean` deletes build artifacts and can slow your next build significantly.
> Use it only when troubleshooting build-state issues.

## Command Details

### `cargo new <name>`

Creates a new package directory.

```bash
cargo new inventory-cli
cargo new inventory-core --lib
```

Use `--lib` when you want a reusable library instead of an executable program.

### `cargo run`

Builds and runs the default binary target.

```bash
cargo run
cargo run -- arg1 arg2
cargo run --bin admin-tool
cargo run --example print_report
```

Arguments after `--` go to your program, not Cargo.

### `cargo test`

Runs unit tests, integration tests, and doctests.

```bash
cargo test
cargo test formats_report
cargo test -- --nocapture
```

`--nocapture` shows output printed during tests. Use it while debugging, not as a
normal habit.

### `cargo clippy`

Runs Clippy, Rust's linter.

```bash
cargo clippy
cargo clippy -- -D warnings
```

`-D warnings` turns warnings into errors. Use it in CI to keep the codebase clean.

### `cargo clean`

Deletes `target/`.

```bash
cargo clean
```

Use when build artifacts are corrupt, disk usage is too high, or you need a truly
fresh build. It is not part of the normal edit/test loop.

## Cross References

- [Cargo Manifest and Config](./cargo-manifest-and-config.md)
- [Testing and CI Cheat Sheet](./testing-and-ci-cheat-sheet.md)
- [Errors, Warnings, and Debugging](./errors-warnings-and-debugging.md)

---

[Reference Index](./README.md) / [Rust](../README.md) / [Home](../../../README.md)