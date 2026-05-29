<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../README.md) / [Rust](../README.md) / [Reference](./README.md)

---

# Cargo Workspaces Reference

> Quick lookup for multi-package Rust repositories. For the full lesson, see
> [Cargo Workspaces And Monorepos](../course/01-getting-started/05-workspaces-and-monorepos.md).

---

## Minimal Workspace

```toml
[workspace]
members = [
    "crates/study-core",
    "apps/study-cli",
]
resolver = "3"
```

Use `resolver = "3"` for Rust 2024 workspaces. Rust 2021 workspaces commonly
use `resolver = "2"`.

---

## Common Layout

```text
study-suite/
  Cargo.toml
  Cargo.lock
  crates/
    study-core/
      Cargo.toml
      src/lib.rs
  apps/
    study-cli/
      Cargo.toml
      src/main.rs
```

Recommended direction:

```text
apps/study-cli -> crates/study-core
```

Avoid circular package design.

---

## Local Path Dependency

`apps/study-cli/Cargo.toml`:

```toml
[dependencies]
study-core = { path = "../../crates/study-core" }
```

Rust import:

```rust
use study_core::normalize_topic;
```

Package names can contain hyphens. Rust identifiers use underscores.

---

## Workspace Commands

| Command | Meaning |
|---|---|
| `cargo check --workspace` | Check all members |
| `cargo test --workspace` | Test all members |
| `cargo build --workspace` | Build all members |
| `cargo run -p study-cli` | Run package named `study-cli` |
| `cargo test -p study-core` | Test one package |
| `cargo clippy --workspace --all-targets --all-features -- -D warnings` | Strict lint all members |

Run workspace commands from the root folder.

---

## Workspace Dependencies

Root `Cargo.toml`:

```toml
[workspace.dependencies]
serde = { version = "1", features = ["derive"] }
```

Member `Cargo.toml`:

```toml
[dependencies]
serde = { workspace = true }
```

Use this when multiple members should share one dependency version.

---

## Notices

| Situation | Recommendation |
|---|---|
| One tiny beginner project | Use one package, not a workspace |
| CLI plus reusable core library | Workspace may help |
| Many members using same dependency | Consider `[workspace.dependencies]` |
| `cargo run` does not know what to run | Use `cargo run -p package-name` |
| Workspace commands ignore a crate | Add it to `members` |

---

[Reference Index](./README.md) / [Rust](../README.md) / [Home](../../../README.md)
