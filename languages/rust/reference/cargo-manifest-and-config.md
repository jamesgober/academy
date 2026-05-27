<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../README.md) / [Rust](../README.md) / [Reference](./README.md)

---

# Cargo Manifest and Config Reference

> `Cargo.toml` tells Cargo what your package is, what it depends on, and how it should be built.

Official docs: [Cargo manifest format](https://doc.rust-lang.org/cargo/reference/manifest.html), [specifying dependencies](https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html), and [Cargo configuration](https://doc.rust-lang.org/cargo/reference/config.html).

## Minimal Application Manifest

```toml
[package]
name = "inventory-cli"
version = "0.1.0"
edition = "2024"

[dependencies]
```

| Field | Required | Description |
|---|---:|---|
| `name` | yes | Package name. Hyphens become underscores in Rust imports. |
| `version` | usually | Package version. Required for publishing. |
| `edition` | recommended | Rust language edition used by the compiler. |
| `[dependencies]` | no | Normal libraries used by your crate. |

## Publish-Ready Package Fields

```toml
[package]
name = "inventory-cli"
version = "0.1.0"
edition = "2024"
rust-version = "1.85"
description = "A small inventory command-line app."
license = "MIT OR Apache-2.0"
repository = "https://github.com/your-name/inventory-cli"
readme = "README.md"
keywords = ["cli", "inventory"]
categories = ["command-line-utilities"]
```

| Field | Use it when | Notice |
|---|---|---|
| `rust-version` | You support a minimum Rust version. | Do not set lower than you actually test. |
| `description` | Publishing or sharing. | Keep it short and factual. |
| `license` | Anyone else may use the code. | Pick a real SPDX expression. |
| `repository` | Source is public or shared. | Helps users audit code. |
| `keywords` | Publishing to crates.io. | Limited count; choose discoverable terms. |
| `categories` | Publishing to crates.io. | Must match crates.io categories. |

## Dependency Forms

```toml
[dependencies]
rand = "0.9"
serde = { version = "1", features = ["derive"] }
tokio = { version = "1", features = ["rt-multi-thread", "macros"] }
```

| Form | Meaning | Tradeoff |
|---|---|---|
| `rand = "0.9"` | Compatible semver dependency. | Simple and common. |
| `{ version = "1", features = [...] }` | Enables crate features. | More explicit; can increase compile time. |
| `{ version = "1", default-features = false }` | Disables default features. | Smaller builds, but you must know what you need. |
| `{ path = "../core" }` | Local dependency. | Good for workspaces; not enough for publishing unless also versioned. |
| `{ git = "https://..." }` | Git dependency. | Useful temporarily; less stable than registry releases. |

## Dependency Sections

```toml
[dependencies]
serde = "1"

[dev-dependencies]
pretty_assertions = "1"

[build-dependencies]
cc = "1"
```

| Section | Available to | Example use |
|---|---|---|
| `[dependencies]` | normal crate code | JSON, CLI parsing, async runtime |
| `[dev-dependencies]` | tests, examples, benches | test helpers |
| `[build-dependencies]` | `build.rs` build scripts | compiling C code |

## Features

Features are named compile-time options.

```toml
[features]
default = ["json"]
json = ["dep:serde", "dep:serde_json"]

[dependencies]
serde = { version = "1", optional = true, features = ["derive"] }
serde_json = { version = "1", optional = true }
```

Run with:

```bash
cargo test --no-default-features
cargo test --features json
```

Notice: features are powerful, but too many feature combinations can make testing
hard. Keep them small and intentional.

## Profiles

Profiles change compiler settings.

```toml
[profile.release]
opt-level = 3
lto = "thin"
codegen-units = 1
```

| Field | Effect | Tradeoff |
|---|---|---|
| `opt-level` | Runtime optimization. | Higher may compile slower. |
| `debug` | Debug info in artifacts. | Useful for profiling; larger output. |
| `lto` | Link-time optimization. | Smaller/faster sometimes; slower builds. |
| `panic = "abort"` | Smaller binaries. | No unwinding; changes panic behavior. |

## Local Cargo Config

Project config lives at `.cargo/config.toml`.

```toml
[build]
target-dir = "target"

[alias]
xtest = "test --all-targets"
xcheck = "check --all-targets"
```

Use aliases for project-specific workflows. Do not hide surprising behavior
behind aliases that teammates cannot understand.

## Cross References

- [Cargo Commands](./cargo-commands.md)
- [Cargo Workspaces](./cargo-workspaces.md)
- [Crates, Libraries, and Ecosystem Lookup](./crates-libraries-and-ecosystem.md)
