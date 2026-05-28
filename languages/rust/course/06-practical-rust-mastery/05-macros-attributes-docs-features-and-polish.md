<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../../README.md) / [Rust](../../README.md) / [Chapter 06](./README.md)

---

# Macros, Attributes, Docs, Features, and Polish

> Rust has small language tools that make projects cleaner once you know what they are doing.

**You will learn:**
- what macros are and why they use `!`
- common built-in macros
- attributes such as `#[derive(...)]`, `#[test]`, and `#[cfg(...)]`
- documentation comments and doctests
- Cargo features at a beginner-safe level
- polish habits before sharing code

**Before this page, you should know:** [Files, CLI Input, Environment, and Useful Programs](./04-files-cli-input-environment-and-useful-programs.md)

---

## Macros Are Code Generators

A macro writes Rust code for you before compilation finishes. Macro calls usually
end with `!`.

```rust
println!("Hello, {}", "Rust");
vec![1, 2, 3];
format!("{} minutes", 45);
```

These look like functions, but they are macros because they accept flexible
syntax and expand into Rust code.

## Common Macros

| Macro | Use |
|---|---|
| `println!` | print line to stdout |
| `eprintln!` | print line to stderr |
| `format!` | build a `String` |
| `vec!` | create a vector |
| `assert!` | test condition |
| `assert_eq!` | test equality |
| `dbg!` | quick debug print with file/line |
| `todo!` | placeholder that panics if reached |
| `unimplemented!` | marker for not-yet-implemented code |
| `panic!` | crash current thread with message |

Use `dbg!` while learning:

```rust
let minutes = 45;
dbg!(minutes);
```

Remove noisy debug macros before committing polished code.

## Attributes

Attributes are metadata written with `#[]`.

Derive traits:

```rust
#[derive(Debug, Clone, PartialEq, Eq)]
pub struct StudyEntry {
    topic: String,
    minutes: u32,
}
```

Mark tests:

```rust
#[test]
fn creates_entry() {
    // test body
}
```

Allow a warning locally:

```rust
#[allow(dead_code)]
fn experimental_helper() {}
```

Use `allow` sparingly. Prefer deleting unused code or wiring it correctly.

## Conditional Compilation

`cfg` means configuration. It includes code only when a condition is true.

```rust
#[cfg(test)]
mod tests {
    // compiled only during tests
}
```

Feature-gated code:

```rust
#[cfg(feature = "json")]
pub fn export_json() {
    // only compiled with the json feature
}
```

Cargo feature:

```toml
[features]
default = []
json = []
```

Run:

```bash
cargo test --features json
```

Do not add features casually. Each feature combination is another thing to test.

## Documentation Comments

Use `///` for public item docs.

```rust
/// A validated study session entry.
///
/// Use [`StudyEntry::new`] to create entries so invalid data is rejected.
#[derive(Debug, Clone, PartialEq, Eq)]
pub struct StudyEntry {
    topic: String,
    minutes: u32,
}
```

Document public functions with purpose, parameters, return value, errors, and an
example when useful. This small complete example shows the documentation shape:

```rust
/// Adds two study durations.
///
/// Use this for totals that are already validated as minutes.
pub fn add_minutes(left: u32, right: u32) -> u32 {
    left + right
}
```

## Doctests

Rust can test examples in documentation.

````rust
/// Adds two numbers.
///
/// ```
/// assert_eq!(study_log::add(2, 3), 5);
/// ```
pub fn add(a: u32, b: u32) -> u32 {
    a + b
}
````

Run:

```bash
cargo test
```

Cargo runs doctests along with normal tests.

## Polish Checklist

Before sharing a Rust project:

```bash
cargo fmt --check
cargo clippy --all-targets --all-features -- -D warnings
cargo test --all-features
cargo doc --open
```

Review:

- public functions have docs
- errors are named and meaningful
- `unwrap()` is not used in normal app flow
- examples compile
- README shows install/run/test commands
- module boundaries are obvious
- `Cargo.toml` metadata is accurate

## Reference Links

- [Cargo Manifest and Config](../../reference/cargo-manifest-and-config.md)
- [Testing and CI Cheat Sheet](../../reference/testing-and-ci-cheat-sheet.md)
- [Errors, Warnings, and Debugging](../../reference/errors-warnings-and-debugging.md)

---

## Recap

- Macros generate code and usually use `!`.
- Attributes add metadata for derives, tests, cfg, and lint behavior.
- Docs are part of the public API.
- Doctests keep examples honest.
- Cargo features are compile-time switches; use them carefully.
- Polish is a workflow, not a vibe.

## Try It Yourself

Add docs and doctests to your study-log library. Then add one optional `json`
feature in `Cargo.toml` and write down which commands you would run to test both
with and without the feature.

---

[**Next ->** Final Capstone: Study Log CLI](./06-final-capstone-study-log-cli.md)  
[**<- Previous** Files, CLI Input, Environment, and Useful Programs](./04-files-cli-input-environment-and-useful-programs.md)
