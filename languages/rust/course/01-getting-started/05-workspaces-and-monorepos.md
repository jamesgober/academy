<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 01](./README.md)

---

# Cargo Workspaces And Monorepos

> A workspace lets several related Rust packages share one command surface,
> one lockfile, and one build directory.

**You will learn:**
- What a Cargo workspace is
- When a workspace helps
- When a workspace is too much
- How to create a small library plus CLI workspace
- How workspace dependencies and commands work

**Before this page, you should know:** [Cargo Workflow Essentials](./04-cargo-workflow.md)

---

## What A Workspace Is

A workspace is a group of packages managed together.

Visual model:

```text
study-suite/
  Cargo.toml             workspace root
  Cargo.lock             shared lockfile
  target/                shared build output
  crates/
    study-core/
      Cargo.toml         package
      src/lib.rs         crate
  apps/
    study-cli/
      Cargo.toml         package
      src/main.rs        crate
```

Root `Cargo.toml`:

```toml
[workspace]
members = [
    "crates/study-core",
    "apps/study-cli",
]
resolver = "3"
```

`resolver = "3"` is the modern resolver used with Rust 2024 projects. If your
organization is using Rust 2021, you may see `resolver = "2"` instead.

---

## When To Use A Workspace

Use a workspace when:

- You have multiple related packages
- A CLI and a library should live together
- Several crates should share one `Cargo.lock`
- You want `cargo test --workspace`
- You are building a monorepo intentionally

Do not start with a workspace when:

- You are building one small beginner project
- You do not understand single-package Cargo yet
- You are splitting code only to look advanced

Beginner rule:

```text
Start with one package.
Move to a workspace when there are real package boundaries.
```

---

## Create A Workspace

Create folders:

```bash
mkdir study-suite
cd study-suite
mkdir crates apps
```

Create root `Cargo.toml`:

```toml
[workspace]
members = [
    "crates/study-core",
    "apps/study-cli",
]
resolver = "3"
```

Create member packages:

```bash
cargo new crates/study-core --lib
cargo new apps/study-cli --bin
```

Final shape:

```text
study-suite/
  Cargo.toml
  crates/
    study-core/
      Cargo.toml
      src/
        lib.rs
  apps/
    study-cli/
      Cargo.toml
      src/
        main.rs
```

---

## Make The CLI Depend On The Library

`apps/study-cli/Cargo.toml`:

```toml
[package]
name = "study-cli"
version = "0.1.0"
edition = "2024"

[dependencies]
study-core = { path = "../../crates/study-core" }
```

`crates/study-core/src/lib.rs`:

```rust
pub fn normalize_topic(topic: &str) -> Result<String, String> {
    let topic = topic.trim();

    if topic.is_empty() {
        return Err("topic cannot be empty".to_string());
    }

    Ok(topic.to_lowercase())
}
```

`apps/study-cli/src/main.rs`:

```rust
use study_core::normalize_topic;

fn main() {
    let topic = normalize_topic(" Rust ").expect("topic should be valid");

    println!("topic: {topic}");
}
```

Notice:

```text
Package name: study-core
Rust import:  study_core
```

Hyphen becomes underscore in Rust code.

---

## Workspace Commands

From the workspace root:

```bash
cargo check --workspace
cargo test --workspace
cargo clippy --workspace --all-targets --all-features -- -D warnings
```

Run a package:

```bash
cargo run -p study-cli
```

Test one package:

```bash
cargo test -p study-core
```

Build everything:

```bash
cargo build --workspace
```

`-p` means package.

---

## Workspace Dependencies

Workspaces can centralize dependency versions.

Root `Cargo.toml`:

```toml
[workspace]
members = [
    "crates/study-core",
    "apps/study-cli",
]
resolver = "3"

[workspace.dependencies]
serde = { version = "1", features = ["derive"] }
```

Member `Cargo.toml`:

```toml
[dependencies]
serde = { workspace = true }
```

This keeps versions consistent across packages.

Use this when multiple workspace members use the same dependency.

---

## Monorepo Habits

Good workspace names:

```text
crates/study-core
apps/study-cli
crates/study-parser
apps/study-server
```

Weak names:

```text
crates/common
crates/misc
crates/stuff
```

Keep boundaries clear:

```text
library package -> reusable rules
binary package  -> app wiring
```

Do not let every crate depend on every other crate.

Healthy dependency direction:

```text
apps/study-cli
        |
        v
crates/study-core
```

Risky dependency tangle:

```text
app -> core -> app
parser -> report -> parser
```

---

## Common Workspace Mistakes

| Mistake | Symptom | Fix |
|---|---|---|
| Running `cargo run` at root with no default binary | Cargo asks which binary/package | Use `cargo run -p package-name` |
| Wrong path dependency | Package cannot find local crate | Check relative path from member `Cargo.toml` |
| Forgetting root `members` | Package ignored by workspace commands | Add member path |
| Package name/import confusion | `use study-core` fails | Use `study_core` in Rust code |
| Workspace too early | Too many folders, little code | Collapse back to one package |

---

## Mini Project: Core Plus CLI

Build the `study-suite` workspace above.

Then add tests in `crates/study-core/src/lib.rs`:

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn trims_and_lowercases_topic() {
        assert_eq!(normalize_topic(" Rust "), Ok("rust".to_string()));
    }

    #[test]
    fn rejects_empty_topic() {
        assert_eq!(normalize_topic(" "), Err("topic cannot be empty".to_string()));
    }
}
```

Run from the workspace root:

```bash
cargo test --workspace
cargo run -p study-cli
```

---

## Chapter Checkpoint

You should now be able to answer:

- What is a Cargo workspace?
- What file defines workspace members?
- What does `cargo test --workspace` do?
- What does `cargo run -p study-cli` mean?
- How does a local path dependency work?
- Why should beginners avoid workspaces too early?
- What does `[workspace.dependencies]` help with?

---

## Recap

- Workspaces group related packages.
- A workspace has one root `Cargo.toml`.
- Members can be libraries or binaries.
- Use `--workspace` to check all members.
- Use `-p` to choose a package.
- Start with one package; split when boundaries are real.

## Try It Yourself

Create a workspace with:

- `crates/math-core` as a library
- `apps/math-cli` as a binary
- One path dependency from CLI to library
- One test in the library

Run everything from the workspace root.

---

[**Next ->** Git And GitHub For Rust Projects](./06-git-and-github-for-rust.md)  
[**<- Previous** Cargo Workflow Essentials](./04-cargo-workflow.md)
