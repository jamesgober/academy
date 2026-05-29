<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 05](./README.md)

---

# Crates And Cargo Packages

> Rust project structure gets much easier when you separate three ideas:
> package, crate, and module.

**You will learn:**
- What a Cargo package is
- What a crate is
- What a module is
- How `src/lib.rs`, `src/main.rs`, and `src/bin/*.rs` fit together
- How `Cargo.toml` controls package metadata and dependencies
- How to choose boundaries for beginner and real-world projects

**Before this page, you should know:** [Chapter 04 Capstone](../04-concurrency-and-async/05-chapter-04-capstone.md)

---

## The Three-Level Map

Rust projects can feel confusing because people say "crate" when they sometimes
mean "package" or "library."

Use this map:

```text
Package
  Cargo-managed project with Cargo.toml

Crate
  One compiled Rust target inside a package

Module
  A namespace inside a crate
```

Visual model:

```text
my-tool/                         Package
  Cargo.toml
  src/
    lib.rs                       Library crate root
    main.rs                      Binary crate root
    parser.rs                    Module used by lib.rs or main.rs
    bin/
      import.rs                  Another binary crate
      export.rs                  Another binary crate
```

One package can contain more than one crate.

One crate can contain many modules.

---

## Package In Plain Language

A package is the Cargo project.

It has a `Cargo.toml`.

Create one:

```bash
cargo new study-tools
cd study-tools
```

Generated shape:

```text
study-tools/
  Cargo.toml
  src/
    main.rs
```

The package name comes from `Cargo.toml`:

```toml
[package]
name = "study-tools"
version = "0.1.0"
edition = "2021"
```

The package is what Cargo builds, tests, and publishes.

---

## Crate In Plain Language

A crate is one compilation unit.

Two common crate types:

```text
Library crate
  Reusable code
  Root file: src/lib.rs

Binary crate
  Executable program
  Root file: src/main.rs or src/bin/name.rs
```

Example package with one library and one binary:

```text
study-tools/
  Cargo.toml
  src/
    lib.rs
    main.rs
```

Cargo sees:

```text
library crate -> src/lib.rs
binary crate  -> src/main.rs
```

The binary can use the library by package name converted to an underscore:

```rust
use study_tools::total_minutes;
```

Package name:

```toml
name = "study-tools"
```

Rust crate import name:

```rust
study_tools
```

Hyphen in package names becomes underscore in Rust code.

---

## Module In Plain Language

A module is a namespace inside a crate.

Modules help organize code:

```text
crate
  parser module
  report module
  model module
```

Example:

```text
src/
  lib.rs
  parser.rs
  report.rs
```

`src/lib.rs`:

```rust
pub mod parser;
pub mod report;
```

`pub mod parser;` means:

```text
Find the parser module in src/parser.rs and make the module public.
```

You will go deeper on modules in the next lesson.

---

## `Cargo.toml` Required Fields

Most beginner packages need:

```toml
[package]
name = "study-tools"
version = "0.1.0"
edition = "2021"

[dependencies]
```

What each field means:

| Field | Required? | Meaning |
|---|---:|---|
| `name` | Yes | Package name |
| `version` | Yes | Package version |
| `edition` | Yes | Rust language edition |
| `[dependencies]` | No, but common | Runtime dependencies |

The `edition` controls which Rust edition rules the crate uses. New beginner
projects should use the latest stable edition supported by the project template
or organization.

---

## Common Optional Fields

For published or team projects:

```toml
[package]
name = "study-tools"
version = "0.1.0"
edition = "2021"
description = "Small tools for tracking study sessions"
license = "MIT OR Apache-2.0"
repository = "https://github.com/example/study-tools"
readme = "README.md"
keywords = ["study", "cli", "learning"]
categories = ["command-line-utilities"]
```

These fields help humans and package indexes understand the project.

Beginner rule:

```text
Local learning project? Keep Cargo.toml small.
Published crate? Add description, license, repository, readme, and metadata.
```

---

## Dependencies

Add a dependency:

```bash
cargo add serde
```

Or edit `Cargo.toml`:

```toml
[dependencies]
serde = "1"
```

Dependency with features:

```toml
[dependencies]
serde = { version = "1", features = ["derive"] }
```

Development-only dependency:

```toml
[dev-dependencies]
pretty_assertions = "1"
```

Build-time dependency:

```toml
[build-dependencies]
cc = "1"
```

Mental model:

```text
[dependencies]       needed by normal library/app code
[dev-dependencies]   needed only for tests, examples, benches
[build-dependencies] needed by build scripts
```

---

## Multiple Binary Crates

If a package has several command-line tools, use `src/bin/`.

```text
study-tools/
  Cargo.toml
  src/
    lib.rs
    bin/
      summarize.rs
      import.rs
      export.rs
```

Run one:

```bash
cargo run --bin summarize
```

Build one:

```bash
cargo build --bin import
```

Each file in `src/bin/` is a separate binary crate.

All of them can use the shared library crate:

```rust
use study_tools::parse_entries;
```

---

## Workspace Preview

A workspace groups multiple packages.

```text
academy-tools/
  Cargo.toml
  crates/
    study-core/
      Cargo.toml
      src/lib.rs
    study-cli/
      Cargo.toml
      src/main.rs
```

Root `Cargo.toml`:

```toml
[workspace]
members = [
    "crates/study-core",
    "crates/study-cli",
]
resolver = "2"
```

Use a workspace when:

- Multiple packages are developed together
- Several apps share internal libraries
- You want one lockfile and one command surface

Do not start with a workspace just to look professional. Start simple, then
split when the project earns it.

---

## Boundary Decisions

Ask:

```text
Is this code reusable?
Does it need independent tests?
Does it have a stable public API?
Will multiple binaries call it?
Will another package depend on it?
```

If yes, it probably belongs in a library crate.

Ask:

```text
Does this code parse CLI args?
Does it print to the terminal?
Does it call process::exit?
Does it wire configuration to library logic?
```

If yes, it probably belongs in a binary crate.

Ask:

```text
Is this code just a section inside the same library?
```

If yes, it probably belongs in a module.

---

## Mini Project: Map A Package

You want to build a study tracker with:

- A CLI app
- A reusable parser
- A report generator
- A second command to import CSV later

Suggested shape:

```text
study-tools/
  Cargo.toml
  src/
    lib.rs
    parser.rs
    report.rs
    main.rs
    bin/
      import_csv.rs
```

Why:

- `Cargo.toml` defines the package.
- `src/lib.rs` is reusable library root.
- `parser.rs` and `report.rs` are modules.
- `main.rs` is the default CLI binary.
- `src/bin/import_csv.rs` is a second binary.

---

## Chapter Checkpoint

You should now be able to answer:

- What is a package?
- What is a crate?
- What is a module?
- What file creates a library crate?
- What file creates the default binary crate?
- Why does `study-tools` become `study_tools` in Rust imports?
- When would you use `src/bin/`?
- When would you use a workspace?

---

## Recap

- A package is a Cargo project with `Cargo.toml`.
- A crate is one compiled target.
- A module is a namespace inside a crate.
- `src/lib.rs` creates a library crate.
- `src/main.rs` creates a binary crate.
- `src/bin/*.rs` creates additional binary crates.
- Keep projects simple until boundaries become useful.

## Try It Yourself

Create a package named `note-tools` with:

- `src/lib.rs`
- `src/main.rs`
- `src/bin/export.rs`
- One module named `format`

Then write down which files are crates and which files are modules.

---

[**Next ->** Modules, Visibility, And Exports](./02-modules-and-visibility.md)  
[**<- Previous** Chapter 04: Concurrency And Async](../04-concurrency-and-async/README.md)
