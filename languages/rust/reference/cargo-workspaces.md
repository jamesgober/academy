<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../README.md) / [Rust](../README.md) / [Reference](./README.md)

---

# Cargo Workspaces Reference

> Quick lookup. For explanation, see [Cargo Workspaces and Monorepos](../course/01-getting-started/05-workspaces-and-monorepos.md).

## At a glance

| Item | Syntax | Purpose |
|------|--------|---------|
| Workspace root | `[workspace]` | Defines shared workspace |
| Members | `members = ["..."]` | Lists crates in workspace |
| Resolver | `resolver = "2"` | Modern dependency resolution |

---

## Minimal workspace root file

```toml
[workspace]
members = ["apps/cli", "crates/core-lib"]
resolver = "2"
```

## Workspace-level commands

```bash
cargo check --workspace
cargo test --workspace
cargo run -p cli
```

> [!IMPORTANT]
> Run workspace commands from the workspace root directory.

> [!WARNING]
> Monorepos add structure complexity. Avoid workspaces for single-crate beginner projects.

---

[Reference Index](./README.md) / [Rust](../README.md) / [Home](../../../README.md)