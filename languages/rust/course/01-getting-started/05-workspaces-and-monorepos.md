<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Rust](../../README.md) · [Chapter 01](./README.md)

</div>

---

# Cargo Workspaces and Monorepos

> Use workspaces when you have multiple related crates that should share tooling and dependency versions.

**You will learn:**
- What a Cargo workspace is
- When to use a workspace (and when not to)
- A safe starter workspace layout

**Before this page, you should know:**
- [Cargo Workflow Essentials](./04-cargo-workflow.md)

---

## What a workspace is

A workspace is a set of crates managed together by one top-level `Cargo.toml`.
It lets you run commands across all member crates with one command.

Example:

```text
rust-workspace/
├── Cargo.toml
├── apps/
│   └── cli/
└── crates/
    └── core-lib/
```

Top-level `Cargo.toml`:

```toml
[workspace]
members = ["apps/cli", "crates/core-lib"]
resolver = "2"
```

## When to use workspaces

Use a workspace when:
- You truly have multiple crates with shared lifecycle.
- You want one lockfile and unified commands.
- You need separate binaries and libraries in one repository.

Do not start with a workspace when:
- You are building one small beginner project.
- You are still learning basic Cargo commands.

> [!WARNING]
> Starting with a monorepo too early increases cognitive load and slows learning.
> Begin with a single crate, then split when there is a clear boundary.

## Monorepo best practices

- Keep crate names explicit (`core-lib`, `api-server`, `cli`).
- Keep boundaries clear: shared domain logic in libraries, app wiring in binaries.
- Run checks from workspace root: `cargo check --workspace`.

## Useful workspace commands

```bash
cargo check --workspace
cargo test --workspace
cargo run -p cli
```

---

## Recap

- Workspaces are for multiple related crates managed together.
- They are powerful, but not always the right starting point.
- Use monorepo patterns intentionally, not by default.

## Try it yourself

Create two crates in one workspace: a library and a CLI that depends on it.

---

[**Next ->** Git and GitHub for Rust Projects](./06-git-and-github-for-rust.md)  
[**<- Previous** Cargo Workflow Essentials](./04-cargo-workflow.md)
