<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../README.md) / [Rust](../README.md) / Reference

---

# Rust Reference

> Quick lookup for Rust syntax, Cargo commands, project configuration, modules, errors, and common standard-library patterns.

Use the course when you are learning a concept for the first time. Use this
reference when you need to remember the shape, parameters, tradeoffs, or command.

## Core Workflow

| Topic | Use it for |
|---|---|
| [Cargo Commands](./cargo-commands.md) | Build, run, test, format, lint, docs, clean, install, and inspect. |
| [Cargo Manifest and Config](./cargo-manifest-and-config.md) | `Cargo.toml`, package fields, dependencies, features, profiles, and `.cargo/config.toml`. |
| [Cargo Workspaces](./cargo-workspaces.md) | Multi-crate repositories and shared dependency management. |
| [Environment Variables and Cargo Config](./environment-variables-and-cargo-config.md) | Runtime env vars and build configuration knobs. |
| [Crates, Libraries, and Ecosystem Lookup](./crates-libraries-and-ecosystem.md) | Where to find crates, docs, examples, and risk signals. |

## Language Lookup

| Topic | Use it for |
|---|---|
| [Modules, Visibility, and Exports](./modules-visibility-and-exports.md) | `mod`, `pub`, `use`, `pub use`, crate paths, and public API design. |
| [Types, Strings, and Collections](./types-strings-and-collections.md) | Integers, floats, bools, chars, `String`, `&str`, arrays, slices, `Vec`, maps, tuples, and structs. |
| [Functions, Methods, Generics, and Traits](./functions-methods-generics-and-traits.md) | Parameters, returns, methods, associated functions, generic `<T>`, trait bounds, and `impl Trait`. |
| [Ownership and Borrowing Cheat Sheet](./ownership-and-borrowing-cheat-sheet.md) | Move, clone, borrow, mutable borrow, lifetimes, and common borrow-checker fixes. |
| [Errors, Warnings, and Debugging](./errors-warnings-and-debugging.md) | Reading compiler output, warnings, `Result`, `Option`, `panic!`, backtraces, and Clippy. |
| [Testing and CI Cheat Sheet](./testing-and-ci-cheat-sheet.md) | Unit tests, integration tests, examples, doctests, and CI quality commands. |

## Risk Notes

| Area | Watch for |
|---|---|
| Public API | `pub` is a promise. Prefer private fields and small exported surfaces. |
| `unwrap()` | Fine in tiny examples, tests, and prototypes; risky in user-facing code. |
| Cloning | `clone()` can be correct, but do not use it to avoid understanding ownership. |
| Dependencies | Check maintenance, docs, license, version, and transitive dependencies before adopting. |
| Async | Async Rust needs a runtime such as Tokio or async-std; `async fn` alone does not run work. |

---

[Rust](../README.md) / [Home](../../../README.md)
