<h1 align="center">
    <img width="99" alt="Rust logo" src="../../../_assets/logos/rust.svg">
    <br>
    <b>Rust</b>
</h1>

[Home](../../../README.md) / [Rust](../README.md) / [Reference](./README.md)

---

# Practical Rust Patterns

> Lookup for the everyday patterns that turn Rust syntax into useful software.

Course chapter: [Practical Rust Mastery](../course/06-practical-rust-mastery/).

## Function Boundary Defaults

| Need | Signature pattern |
|---|---|
| read text | `fn f(input: &str)` |
| store text | `fn new(input: impl Into<String>) -> Self` |
| read list | `fn f(items: &[T])` |
| mutate caller value | `fn f(value: &mut T)` |
| maybe no value | `fn f(...) -> Option<T>` |
| expected failure | `fn f(...) -> Result<T, E>` |
| app entry flow | `fn run() -> Result<(), Box<dyn Error>>` |

## CLI Shape

```rust
fn main() {
    if let Err(error) = run() {
        eprintln!("error: {error}");
        std::process::exit(1);
    }
}
```

Keep `main` small. Put real logic in testable functions.

## File I/O

```rust
let contents = std::fs::read_to_string(path)?;
std::fs::write(path, contents)?;
```

File operations return `Result` because missing files, permissions, directories,
and invalid UTF-8 are normal failure cases.

## Parsing

```rust
let minutes = raw.trim().parse::<u32>()?;
```

Map errors when you want your own error type:

```rust
let minutes = raw
    .trim()
    .parse::<u32>()
    .map_err(|_| ParseEntryError::InvalidMinutes)?;
```

## Collections

```rust
let mut totals = std::collections::HashMap::new();

for entry in entries {
    *totals.entry(entry.topic().to_owned()).or_insert(0) += entry.minutes();
}
```

## Iterators

```rust
let total: u32 = entries.iter().map(StudyEntry::minutes).sum();
let long = entries.iter().filter(|entry| entry.minutes() >= 60);
let names = entries.iter().map(|entry| entry.topic()).collect::<Vec<_>>();
```

## Threads for Background Work

Rust's equivalent to "background work" is often a thread for CPU work or async
tasks for I/O work.

```rust
let handle = std::thread::spawn(|| {
    (1..=1_000).sum::<u32>()
});

let total = handle.join().expect("worker thread panicked");
```

Use threads when work can safely run in parallel. Use channels, mutexes, or
message passing when threads need to communicate.

## Macros and Attributes

```rust
println!("hello");
let values = vec![1, 2, 3];

#[derive(Debug, Clone, PartialEq, Eq)]
struct Item;

#[cfg(test)]
mod tests {}
```

## Polish Commands

```bash
cargo fmt --check
cargo clippy --all-targets --all-features -- -D warnings
cargo test --all-features
cargo doc --open
```

## Cross References

- [Cargo Commands](./cargo-commands.md)
- [Types, Strings, and Collections](./types-strings-and-collections.md)
- [Functions, Methods, Generics, and Traits](./functions-methods-generics-and-traits.md)
- [Errors, Warnings, and Debugging](./errors-warnings-and-debugging.md)
