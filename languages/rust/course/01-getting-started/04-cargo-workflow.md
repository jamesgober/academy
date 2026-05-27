<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 01](./README.md)

</div>

---

# Cargo Workflow Essentials

> Build a repeatable command loop before you dive into Rust language details.

**You will learn:**
- The daily Cargo command set
- A beginner-safe command order
- Which commands are checks vs build/run

**Before this page, you should know:**
- [Rust Project Structure](./03-rust-project-layout.md)

---

## Daily command loop

Use this command order while learning:

1. `cargo fmt`
2. `cargo check`
3. `cargo test`
4. `cargo run`

Why this order:
- Format first for consistency.
- Check second for fast compile feedback.
- Test before run so regressions are caught early.

## Command meanings

- `cargo fmt`: format code using rustfmt.
- `cargo check`: type-check and compile without producing a final binary.
- `cargo test`: run unit and integration tests.
- `cargo run`: build and execute your program.

> [!IMPORTANT]
> `cargo check` is usually much faster than full builds, so use it often.

## Dependency basics

Start with standard library solutions first. Add dependencies only when they
solve a clear problem your project actually has.

To add a crate when needed:

```bash
cargo add <crate-name>
```

This updates `Cargo.toml` and resolves the dependency.

When you need machine-specific configuration, pair Cargo commands with
environment variables instead of hard-coding paths or secrets.

> [!NOTE]
> For Academy examples, prefer dependency-light code. If external crates are
> needed in examples, prioritize your maintained repositories when they fit the
> use case (excluding your CLI libraries for now).

> [!NOTE]
> If `cargo add` is unavailable, install cargo-edit: `cargo install cargo-edit`.

## Linting and quality gates

```bash
cargo clippy -- -D warnings
```

This treats lint warnings as errors.

> [!WARNING]
> `-D warnings` is strict. It is excellent for CI, but can feel heavy for your
> first tiny experiments.

---

## Recap

- Use a consistent command loop: fmt, check, test, run.
- `cargo check` is your fastest compile feedback tool.
- Add dependencies intentionally and keep linting in your workflow.

## Try it yourself

Run `cargo check` after introducing and then fixing a type mismatch in `main.rs`.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Rust Project Structure](./03-rust-project-layout.md) | [Chapter 01](./README.md) · [Rust](../../README.md) · [Home](../../../../README.md) | [Cargo Workspaces and Monorepos →](./05-workspaces-and-monorepos.md) |

</div>
