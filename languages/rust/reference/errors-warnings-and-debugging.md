<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../README.md) / [Rust](../README.md) / [Reference](./README.md)

---

# Errors, Warnings, and Debugging Reference

> Lookup for reading compiler output, warnings, panics, `Result`, `Option`, and Clippy.

## Compiler Error Shape

Rust errors usually contain:

```text
error[E0382]: borrow of moved value: `name`
 --> src/main.rs:4:22
  |
2 |     let name = String::from("Ada");
3 |     let other = name;
  |                 ---- value moved here
4 |     println!("{name}");
  |                      ^ value borrowed here after move
```

Read in this order:

1. error code and summary
2. file and line
3. marked source span
4. explanation notes
5. suggested fix, if present

Use:

```bash
rustc --explain E0382
```

## Warning Shape

```text
warning: unused variable: `count`
 --> src/main.rs:2:9
  |
2 |     let count = 3;
  |         ^^^^^ help: if this is intentional, prefix it with an underscore: `_count`
```

Warnings mean the code compiled, but Rust found something suspicious or unused.
Do not ignore warnings in course or production code.

## `Result<T, E>`

Use `Result` for expected failure.

```rust
fn divide(total: i32, by: i32) -> Result<i32, String> {
    if by == 0 {
        return Err(String::from("cannot divide by zero"));
    }

    Ok(total / by)
}
```

| Method/operator | Use | Risk |
|---|---|---|
| `?` | return early on error | function must return compatible `Result` |
| `unwrap()` | get value or panic | avoid in user-facing code |
| `expect("message")` | panic with context | okay in examples/tests for impossible failures |
| `map()` | transform success | can become hard to read if chained too far |
| `map_err()` | transform error | useful for custom error types |

## `Option<T>`

Use `Option` when a value may be absent.

```rust
fn first_name(names: &[String]) -> Option<&str> {
    names.first().map(String::as_str)
}
```

| Variant | Meaning |
|---|---|
| `Some(value)` | value exists |
| `None` | value does not exist |

## Panics and Backtraces

A panic means the program hit an unrecoverable failure.

```bash
RUST_BACKTRACE=1 cargo run
```

Use backtraces to find where a panic started. Fix the first relevant frame in
your code, not the deepest standard-library frame.

## Clippy

Clippy is Rust's linter.

```bash
cargo clippy
cargo clippy -- -D warnings
```

Common Clippy messages point out needless clones, inefficient patterns, confusing
conditionals, or more idiomatic standard-library methods.

## Debugging Checklist

| Symptom | First move |
|---|---|
| borrow checker error | identify who owns the value and where it moved |
| private item error | check `pub`, parent module visibility, and import path |
| trait bound error | find which method/operator requires the missing trait |
| panic | rerun with `RUST_BACKTRACE=1` |
| test failure | run the specific test with `-- --nocapture` |
| dependency error | check crate name, version, features, and network access |

## Cross References

- [Ownership and Borrowing Cheat Sheet](./ownership-and-borrowing-cheat-sheet.md)
- [Modules, Visibility, and Exports](./modules-visibility-and-exports.md)
- [Cargo Commands](./cargo-commands.md)
